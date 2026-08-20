---
schema_version: "1.0.0"
document_id: "18f0fcbd225a2083631e6a90300042c97383b5c809a4ba1f21ae83288cbce76c"
company_key: "agora-inc-american-depositary-shares"
company: "Agora Inc."
source_id: "agora-inc-american-depositary-shares-news-import-bf245f251fd4"
canonical_url: "https://www.agora.io/en/blog/building-a-multiplayer-turn-based-game-with-agora-rtc-and-ai-voice-agents"
published_at: "2026-04-16T00:00:00+00:00"
first_seen_at: "2026-07-23T01:17:52.872297+00:00"
fetched_at: "2026-07-28T21:25:38.770750+00:00"
content_hash: "sha256:a375c1da751e405b1d2be859749deaeecd0397f84ef08d739183ee8e05f12c9a"
---

# Building a Multiplayer Turn-Based Game with Agora RTC and AI Voice Agents

Real-time communication (RTC) is typically associated with video and audio experiences. However, the same infrastructure can also be used to exchange structured data that drives application logic.


In many real-time systems, keeping game state in sync across clients requires additional layers such as WebSockets or backend services to manage state synchronization. In this project, I explore an alternative approach — using datastream messaging within Agora’s RTC SDK to handle lightweight game state alongside media streams, reducing the need for separate infrastructure in specific scenarios.


It is important to note that RTC datastream messaging is not intended for production-grade signaling. Since delivery is not guaranteed or strictly ordered, it is best suited for non-critical data such as transcripts or auxiliary state. For scalable and reliable signaling, a dedicated messaging layer such as a signaling service should be used.


With that constraint in mind, I intentionally keep this implementation minimal — using the Agora WebSDK, basic frontend technologies, and lightweight serverless functions to simulate backend behavior. The goal is not to present a production-ready architecture, but to demonstrate how these primitives can be combined into a functional system.


This guide walks through building a two-player Battleship game that uses Agora for three things simultaneously:


1. Video/audio communication between players
2. Game state synchronization via datastream messages through the existing WebRTC connection
3. Voice-controlled AI agents that respond to player commands (if desired!)


Along the way, I highlight where this approach works well, where it breaks down, and how it can be extended.


## What We’re Building


A browser-based Battleship game where:


- Players can start/join/view games
- Two players see and hear each other while playing
- Each player has their own AI agent listening in separate channels
- Players attack by speaking coordinates to an AI agent (“Attack B4”)
- Players can ask their respective AI Agent to choose for them (“Choose for me!”)
- The game can still be played via mouse clicks, with the AI Agent’s reacting to the player’s mouse input
- Game state (attacks, hits, misses) syncs through Agora’s datastream channel
- Spectators can join and watch live gameplay


## The Core Problem: Syncing Turn-Based Game State


In a turn-based game, you need every player to agree on:


- Whose turn it is
- What moves were made
- What the board looks like
- When the game ends


A basic approach is to poll a server at regular intervals.


The more efficient approach: server pushes/pulls updates via WebSocket.


The fun (but not necessarily correct!) Agora approach we’ll be using here: *use the same RTC connection that’s already open for video and audio* .


## Why Agora’s Datastream Messaging Works Here


Agora’s WebSDK client’s` sendStreamMessage()` API lets you broadcast data messages into the existing RTC channel. For a Battleship move (row, column, result), the payload is typically small (approximately 50 bytes), which is suitable given the low message frequency. This API does not replace a server or service that tracks multiplayer game state, but it does allow us to emulate having a backend to relay states with minimal effort.


Here’s what some of the message structure looks like that we’ll be using here:


```text
// Attack message sent when a player makes a move
const   msgObj = {
type  :   "attack"  ,
row  :   3  ,
col  :   7
};
client.sendStreamMessage(  JSON  .stringify(msgObj));


// Attack result sent back by the defender
const   resultMsg = {
type  :   "attack_result"  ,
row  :   3  ,
col  :   7  ,
isHit  :   true  ,
isGameOver  :   false
};
client.sendStreamMessage(  JSON  .stringify(resultMsg));
```


The main RTC client for each Player listens for these messages:


```text
client.on(  "stream-message"  , handleStreamMessage);
```


```text
function     handleStreamMessage  (  uid, msgData  )   {
const   decoder =   new   TextDecoder();
const   msgStr = decoder.decode(msgData);
const   msg =   JSON  .parse(msgStr);


switch   (msg.type) {
case     "attack"  :
handleAttack(msg);
break  ;
case     "attack_result"  :
handleAttackResult(msg);
break  ;
case     "ready"  :
handlePlayerReady();
break  ;
}
}
```


This pattern — message types with payloads — gives you a simple RPC-style system. This eliminates the need for a dedicated WebSocket server.


## Architecture: Multi-Channel Design


Here’s where things get interesting. This game doesn’t use one Agora channel. It uses three channels per game:


1. **Main game channel** (` battleship_123456` )


- Both players join as hosts
- Video/audio/data messages flow here
- Audience members join as audience role


2. **Agent A channel** (` battleship_123456_agenta` )


- Player A publishes audio here, when it’s their turn
- Agent A joins and listens
- Agent A responds with voice TTS
- Player B subscribes here, but only to Agent A. Player B is already receiving Player A’s audio from the main game channel.


3. Agent B channel (` battleship_123456_agentb` )


- Player B publishes audio here
- Agent B joins and listens
- Agent B responds with voice commands
- Player A subscribes here, but only to Agent B. Player Ais already receiving Player B’s audio from the main game channel.


Why separate channels for agents? Isolation. Each agent needs to hear only its player’s voice commands, not the opponent’s. If both agents were in the main channel, they’d interfere with each other and respond to the wrong player’s commands.


## Code: Setting Up Multiple Clients


```text
// Main game client for video/audio/data
client = AgoraRTC.createClient({   mode  :   "live"  ,   codec  :   "vp8"  ,   role  :   "host"   });
await   client.join(AGORA_APP_ID, currentChannelName,   null  ,   "PlayerA"  );
await   client.publish([localAudioTrack, localVideoTrack]);
```


```text
// Agent client for voice commands
agentClient = AgoraRTC.createClient({   mode  :   "live"  ,   codec  :   "vp8"  ,   role  :   "host"   });
await   agentClient.join(AGORA_APP_ID, currentAgentChannelName,   null  ,   "PlayerA"  );
await   agentClient.publish(localAgentAudioTrack);
```


Each client is independent. You manage their lifecycles separately, handle events separately, but they share the same user context. This is powerful because it lets you segment communication by purpose.


## The Game Loop: Attack and Response Flow


Let’s trace what happens when Player A attacks:


## Step 1: Player Speaks Command


```text
// Player says "Attack B4" into their microphone
// Agent hears this via the agent channel
// Agent processes the transcript and extracts coordinates
```


The agent’s transcript handler parses the spoken text:


```text
function     handleAgentStreamMessage  (  uid, msgData  )   {
const   messageDataJson =   JSON  .parse(messageData);


if   (messageDataJson.text.includes(  "Acknowledged"  )) {
// Agent confirmed valid coordinates
const   match = messageDataJson.text.match(  /([A-J])(1[0]|[1-9])/  );
if   (match) {
const   [_, letter, number] = match;
const   row = letter.charCodeAt(  0  ) -   'A'  .charCodeAt(  0  );
const   col =   parseInt  (number) -   1  ;
attackCell(row, col, letter, number);
}
}
}
```


## Step 2: Attack Message Sent


```text
function     attackCell  (  row, col  )   {
if   (!isMyTurn || enemyBoardState[row][col] !==   0  ) {
showNotification(  "Invalid move!"  );
return  ;
}
```


```text
const   msgObj = {   type  :   "attack"  , row, col };
client.sendStreamMessage(  JSON  .stringify(msgObj));


// Immediately switch turns locally
isMyTurn =   false  ;
updateStatus(  "Waiting for opponent..."  );
}
```


Notice the optimistic turn switch. Player A assumes the message will arrive, so they disable their controls immediately. This prevents double-moves.


## Step 3: Opponent Receives and Processes


```text
function     handleAttack  (  msg  )   {
// Check if the attack hit a ship
const   isHit = myShips[msg.row][msg.col];
myBoardState[msg.row][msg.col] = isHit ?   1   :   2  ;
renderMyBoard();


// Check for game over
const   totalHits = myBoardState.flat().filter(  cell   =>   cell ===   1  ).length;
const   isGameOver = totalHits === TOTAL_SHIP_CELLS;


// Send result back
const   resultMsg = {
type  :   "attack_result"  ,
row  : msg.row,
col  : msg.col,
isHit,
isGameOver
};
client.sendStreamMessage(  JSON  .stringify(resultMsg));


if   (!isGameOver) {
isMyTurn =   true  ;   // Now it's my turn
updateStatus(  "Your turn!"  );
}
}
```


## Step 4: Original Attacker Receives Result


```text
function     handleAttackResult  (  msg  )   {
// Update my view of the enemy's board
enemyBoardState[msg.row][msg.col] = msg.isHit ?   1   :   2  ;
renderEnemyBoard();


if   (msg.isGameOver) {
showGameOver(  true  );   // I won!
return  ;
}


// Result received, but it's opponent's turn now
isMyTurn =   false  ;
updateStatus(  "Waiting for opponent..."  );
}
```


This four-step flow ensures both clients stay synchronized. The key insight: the defender is the source of truth. Player A sends an attack, but Player B determines if it was a hit. Player A must trust Player B’s response.


## Handling Race Conditions


What happens if both players click attack at the same moment?


The Problem: Without a central server acting as referee, you can’t prevent race conditions through locking. Both players might think it’s their turn.


The Solution: Establish a canonical turn order at game start, then trust the message sequence.


```text
function     startGame  (  )   {
bothPlayersReady =   true  ;
// Player A always goes first
isMyTurn = isPlayerA;
updateStatus(isMyTurn ?   "Your turn!"   :   "Waiting for opponent..."  );
}
```


When a player receives an attack while thinking it’s their turn, the` handleAttack` function still processes it correctly because it doesn't check` isMyTurn` . It just responds. This makes the game eventually consistent—even if both players briefly think it's their turn, the message sequence will resolve the conflict.


Could both players send attacks simultaneously? Yes. But because each player only updates their *opponent’s* board based on results they receive, not results they send, the game stays consistent. Player A’s attack updates Player B’s board, and vice versa. They never touch the same data structure.


## Voice Agent Integration


The AI agents are the most impressive part. Each player gets a voice assistant that:


- Listens for “Attack \[coordinates\]” commands
- Validates the coordinates
- Confirms actions with voice feedback
- Can suggest moves when asked “Choose for me”


## Agent Lifecycle


Agents don’t start when players join. They start when both players finish ship placement:


```text
function     handlePlayerReady  (  )   {
otherPlayerReady =   true  ;


if   (!isPlacingShips) {
bothPlayersReady =   true  ;


// NOW start the agents
if   (isPlayerA) {
startAgent(  "AgentA"  , currentAgentChannelName,   "AgentA"  ,   "PlayerA"  ,
"You are acting as a commentator for a game of Battleship..."  ,
"Welcome to Battleship Agora Player A!"  );
}   else   {
startAgent(  "AgentB"  , currentAgentChannelName,   "AgentB"  ,   "PlayerB"  ,
"You are acting as a commentator for a game of Battleship..."  ,
"Welcome to Battleship Agora Player B!"  );
}


isMyTurn = isPlayerA;
updateStatus(isMyTurn ?   "Your turn!"   :   "Waiting for opponent..."  );
}
}
```


Why wait? Because you don’t want agents listening during ship placement. They might accidentally interpret placement instructions as attack commands.


## Agent Lambda Functions


The agents aren’t triggered directly from the browser. We’re just going to call serverless functions which have the API keys needed to start a complete ConvoAI Agent. The serverless functions themselves are just[Agora’s ConvoAI RESTful API calls](https://docs.agora.io/en/conversational-ai/rest-api/agent/join) :


```text
async     function     startAgent  (  name, chan, uid, remoteUid, prompt, message  )   {
const   url =   <  LAMBDA     FUNCTION     URL     OR     API     GATEWAY     URL  >


const reqBody = {
agentname: name,
channel: chan,
agentuid: uid,
agentrtmuid: chan,
rtmflag: true,
remoteuid: remoteUid,
prompt: prompt,
message: message
};


const resp = await fetch(url, {
method: "POST",
headers: {
"Accept": "application/json",
"Content-Type": "application/json"
},
body: JSON.stringify(reqBody)
});


const data = await resp.json();
myAgentsId = data.agent_id; // Save for later cleanup
}
```


The Lambda function spins up an agent that:


1. Joins the specified Agora channel
2. Subscribes to the player’s audio
3. Runs speech-to-text on incoming audio (using Agora’s ARES service, but any supported ASR can be used)
4. Processes commands via GPT-4o-mini (any supported model can be used)
5. Generates text-to-speech responses (using Azure here)
6. Publishes TTS audio and user/assistant.transcript datastream messages back to the channel


## Agent Message Protocol


Agora ConvoAI Voice Agents can send transcripts back via Agora’s RTM (Real-Time Messaging) or RTC datastream messages. Here we opt for the datastream variety, so that we can achieve our goal of just using the WebSDK, nothing more. The messages arrive chunked to handle size limits:


```text
function     handleAgentStreamMessage  (  uid, msgData  )   {
let   [messageId, messagePart, messageChunks, messageData] =
new   TextDecoder().decode(msgData).split(  "|"  );


messageData = atob(messageData);   // Base64 decode


// Reconstruct chunked messages
messagesMap.set(messageId,
messagesMap.get(messageId) ?
messagesMap.get(messageId) + messageData :
messageData
);


// Wait for all chunks
if   (  parseInt  (messagePart) ===   parseInt  (messageChunks)) {
const   fullMessage = messagesMap.get(messageId);
messagesMap.delete(messageId);
processAgentMessage(fullMessage);
}
}
```


This chunking protocol handles large transcripts. Each chunk contains:


- ` messageId` : Unique ID for this message
- ` messagePart` : Which chunk (1, 2, 3...)
- ` messageChunks` : Total chunks
- ` messageData` : Base64-encoded payload


## Parsing Voice Commands


Once you have the full transcript, extract coordinates:


```text
if   (messageDataJson.text.includes(  "Acknowledged"  )) {
// Agent uses "Acknowledged" prefix for valid commands
const   match = messageDataJson.text.match(  /([A-J])(1[0]|[1-9])/  );
if   (match) {
const   [_, letter, number] = match;
const   row = letter.charCodeAt(  0  ) -   'A'  .charCodeAt(  0  );
const   col =   parseInt  (number) -   1  ;
attackCell(row, col, letter, number);
}   else   {
showNotification(  "Invalid coordinates! Try again."  );
}
}
```


The regex` /(\[A-J\])(1\[0\]|\[1-9\])/` matches:


- A letter A-J (rows)
- A number 1–10 (columns, with special handling for “10”)


This parsing happens client-side, not in the agent. The agent’s job is to confirm it heard a valid attack command (“Acknowledged firing on B4”). The client then extracts coordinates and executes the attack.


## Volume Control: Whose Agent Should You Hear?


Both agents are speaking, but you should only hear your own agent clearly:


```text
// Player A's agent channel
await   agentClient.publish(localAgentAudioTrack);
localAgentAudioTrack.setVolume(  100  );   // Full volume for Player A
```


```text
// Player B's agent channel
await   agentClient.join(AGORA_APP_ID, currentAgentChannelName,   null  ,   "PlayerB"  );
await   agentClient.publish(localAgentAudioTrack);
localAgentAudioTrack.setVolume(  0  );   // Muted for Player B during opponent's turn
```


Volume adjusts based on turn:


```text
function     handleAttack  (  msg  )   {
// Opponent attacked me, now it's my turn
isMyTurn =   true  ;
localAgentAudioTrack.setVolume(  100  );   // Unmute my agent
}
```


```text
function     attackCell  (  row, col  )   {
// I'm attacking, switching to opponent's turn
isMyTurn =   false  ;
localAgentAudioTrack.setVolume(  0  );   // Mute my agent
}
```


This creates natural turn-taking, but more importantly keeps one user’s Agent from hearing voice input from the other user. This way both Players can still talk to each other during the game, without triggering the off-turn player’s agent. Your agent listens and speaks when it’s your turn, and stays quiet otherwise.


## Audience Mode: Spectator View


The game supports spectators who can join mid-game and watch both boards. This is trickier than it sounds.


## Joining as Audience


```text
async     function     joinAsAudience  (  chName  )   {
// Create main client as audience role
await   client.join(AGORA_APP_ID, chName,   null  , audienceId);


// Join both agent channels to hear both agents
agentClient = AgoraRTC.createClient({   mode  :   "live"  ,   codec  :   "vp8"  ,   role  :   "audience"   });
await   agentClient.join(AGORA_APP_ID, currentAgentAChannelName,   null  , audienceId);


audienceSpecialClient = AgoraRTC.createClient({   mode  :   "live"  ,   codec  :   "vp8"  ,   role  :   "audience"   });
await   audienceSpecialClient.join(AGORA_APP_ID, currentAgentBChannelName,   null  , audienceId);


// Send join notification so players can send board state
const   msgObj = {   type  :   "audience-joined"   };
client.sendStreamMessage(  JSON  .stringify(msgObj));


// Switch to audience role (no publishing)
await   client.setClientRole(  "audience"  );
}
```


Audience members need three clients:


1. Main channel client (for game messages and video)
2. Agent A channel client (to hear Agent A)
3. Agent B channel client (to hear Agent B)


They join as “audience” role in agent channels, which means they can subscribe but not publish.


## Board State Synchronization


When an audience member joins, they send an` audience-joined` message. Both players respond by broadcasting their current board state:


```text
function     handleAudienceJoined  (  )   {
if   (client) {
const   boardState = {
type  :   "board-state"  ,
isPlayerA  : isPlayerA,
board  : myBoardState,
ships  : myShips
};
client.sendStreamMessage(  JSON  .stringify(boardState));
}
}
```


The audience receives both board states and renders them:


```text
function     handleAudienceStreamMessage  (  uid, msgData  )   {
const   msg =   JSON  .parse(msgStr);


if   (msg.type ===   "board-state"  ) {
if   (msg.isPlayerA) {
myBoardState = msg.board;
myShips = msg.ships;
renderMyBoard();
}   else   {
enemyBoardState = msg.board;
myShips2 = msg.ships;
renderMyBoard2();
}
}
}
```


This approach works because:


1. Audience members don’t modify game state, only render it
2. Players keep audience updated on every move via` board-state` messages
3. Late joiners get caught up by the initial` audience-joined` handshake


## Ship Placement: Pre-Game State


Before the game starts, players place ships on their board. This phase has different rules:


```text
function     startShipPlacement  (  )   {
isPlacingShips =   true  ;
currentShipType =   0  ;
currentShipOrientation =   'horizontal'  ;


// Enable click handlers on player's own board
const   cells = myBoardEl.querySelectorAll(  "div"  );
cells.forEach(  (  cell, idx  ) =>   {
const   row =   Math  .floor(idx /   10  );
const   col = idx %   10  ;
cell.onclick =   () =>   placeShip(row, col);
cell.onmouseover =   () =>   showPlacementPreview(row, col,   true  );
cell.onmouseout =   () =>   showPlacementPreview(row, col,   false  );
});
}
```


Ships are placed sequentially — you must place all Destroyers before moving to Cruisers, etc. This simplifies UI logic:


```text
function     placeShip  (  row, col  )   {
if   (!canPlaceShip(row, col)) {
showNotification(  "Can't place ship here!"  );
return  ;
}


const   ship = SHIPS[currentShipType];


// Mark cells as occupied
for   (  let   i =   0  ; i < ship.length; i++) {
const   r = currentShipOrientation ===   'horizontal'   ? row : row + i;
const   c = currentShipOrientation ===   'horizontal'   ? col + i : col;
myShips[r][c] =   true  ;
}


SHIPS[currentShipType].count--;
if   (SHIPS[currentShipType].count ===   0  ) {
currentShipType++;   // Move to next ship type
}
shipsToPlace--;


if   (shipsToPlace ===   0  ) {
finishPlacement();
}
}
```


## Ready State Synchronization


When a player finishes placement, they send a` ready` message:


```text
function     finishPlacement  (  )   {
isPlacingShips =   false  ;


const   readyMsg = {   type  :   "ready"   };
client.sendStreamMessage(  JSON  .stringify(readyMsg));


if   (otherPlayerReady) {
startGame();   // Both ready, begin!
}   else   {
updateStatus(  "Waiting for opponent to finish placing ships..."  );
}
}
```


The game doesn’t start until both players are ready. This prevents one player from being attacked while still placing ships.


## Error Handling and Edge Cases


## Reconnection


If a player loses connection, Agora’s SDK handles most of the heavy lifting:


```text
client.on(  "connection-state-change"  ,   (  curState, prevState, reason  ) =>   {
console  .log(  `Connection state:   ${prevState}   ->   ${curState}   (  ${reason}  )`  );


if   (curState ===   "DISCONNECTED"  ) {
updateStatus(  "Connection lost. Attempting to reconnect..."  );
}   else     if   (curState ===   "CONNECTED"  ) {
updateStatus(  "Reconnected!"  );
// Re-sync game state by requesting board updates
const   msgObj = {   type  :   "audience-joined"   };   // Reuse audience logic
client.sendStreamMessage(  JSON  .stringify(msgObj));
}
});
```


This isn’t implemented in the current code, but it’s worth adding for production use.


## Message Delivery Guarantees


Agora’s data stream channel uses UDP transport, which does not guarantee delivery or ordering. Applications requiring reliability must implement acknowledgment logic at the application layer. Messages can be lost or arrive out of order. For a turn-based game, this is mostly fine because:


1. Attacks are confirmed by results — if Player B never receives Player A’s attack, Player A won’t get a result, and they’ll know something went wrong
2. Game state is synchronized frequently (every move)
3. The worst case is one lost move, not complete desynchronization


If you need guaranteed delivery, consider:


- Adding sequence numbers to messages
- Implementing ACK/NACK protocol
- Using Agora RTM (Real-Time Messaging) instead of data messages


## Invalid Moves


The game validates moves on both client and server side (remember, both players act as validators):


```text
function     attackCell  (  row, col  )   {
if   (!isMyTurn) {
showNotification(  "Not your turn!"  );
agentSpeak(myAgentsId,   "Wait your turn!"  );
return  ;
}


if   (enemyBoardState[row][col] !==   0  ) {
showNotification(  "Cell already attacked!"  );
agentSpeak(myAgentsId,   "That area is already decimated!"  );
return  ;
}


// Move is valid, proceed
const   msgObj = {   type  :   "attack"  , row, col };
client.sendStreamMessage(  JSON  .stringify(msgObj));
}
```


Even if Player A bypasses these checks (by modifying client code), Player B’s` handleAttack` function will process the move correctly based on *their* board state. Player A can't force a hit where there's no ship.


## Performance Considerations


## Message Size Limits


Agora’s RTC SDK’s sendStreamMessage() API supports a maximum payload of 1 KB per message and a send rate of up to 30 messages per second. It is better to stay within the limits.


```text
// Good: ~50 bytes
{   type  :   "attack"  ,   row  :   3  ,   col  :   7   }
```


```text
// Bad: ~3KB
{
type  :   "attack"  ,
row  :   3  ,
col  :   7  ,
attackerId  :   "player_a_12345"  ,
timestamp  :   "2024-01-15T10:30:00.000Z"  ,
metadata  : { ... },
fullBoardState  : [...]   // Don't send unnecessary data
}
```


Send only what’s needed. Board state is 100 cells × 1 byte = 100 bytes. Ship positions are similar. You have plenty of headroom.


## Rendering Performance


Updating the board on every message could cause flicker:


```text
function     renderMyBoard  (  )   {
const   cells = myBoardEl.querySelectorAll(  "div"  );


cells.forEach(  (  cell, idx  ) =>   {
const   row =   Math  .floor(idx /   10  );
const   col = idx %   10  ;
const   val = myBoardState[row][col];
const   hasShip = myShips[row][col];


// Only update cell className if it changed
const   newClass =   `h-7 w-7 border border-gray-600   ${
val ===   0   ? (hasShip ?   "bg-yellow-500"   :   "bg-gray-700"  ) :
val ===   1   ?   "bg-red-500"   :   "bg-blue-500"
}  `  ;


if   (cell.className !== newClass) {
cell.className = newClass;
}
});
}
```


This approach checks if the className changed before updating, preventing unnecessary reflows. For a 10×10 board, this is overkill, but it’s a good habit for larger games.


## Agent Response Latency


Voice commands introduce measurable latency across several processing stages. The following are representative estimates; actual values will vary based on network conditions, ASR provider performance, and model response time:


1. Player speaks — utterance duration (~1–2 seconds)


2. Audio streams to agent (~50–100ms)


3. Speech-to-text processing (~300–500ms, provider-dependent)


4. LLM processes command (~500ms–2 seconds, model-dependent)


5. Text-to-speech generation (~300–500ms)


6. Audio streams back to client (~50–100ms)


These stages are sequential, so end-to-end latency accumulates across all steps. Developers should measure latency in their own deployment environment before drawing conclusions about user experience impact.


To reduce latency, consider the following:


· Use a lower-latency ASR provider. Services such as Deepgram offer streaming transcription with significantly lower processing times than batch ASR.


· Stream TTS output. Rather than waiting for the full audio clip to generate, stream and play TTS audio progressively as it is produced.


· Reduce prompt verbosity. Shorter, more direct system prompts reduce token processing time on the LLM side.


· Select a faster model. If the use case does not require GPT-4-class reasoning, a smaller or distilled model will reduce inference latency meaningfully.


Note that mouse-click input remains available as a parallel interaction method. Voice control is an enhancement to the interaction model, not a dependency.


## Extending This Pattern


This architecture works for any turn-based game:


## Chess


- Moves:` { type: "move", from: "e2", to: "e4" }`
- State: 64 cells, piece positions
- Agents: “Move pawn to e4” → validates and executes


## Poker


- Moves:` { type: "bet", amount: 50 }` ,` { type: "fold" }`
- State: cards, pot, player chips
- Agents: “I’ll raise 50” → processes bet logic


## Tic-Tac-Toe


- Moves:` { type: "mark", row: 1, col: 1 }`
- State: 9 cells
- Agents: “Bottom right” → converts to coordinates


The pattern scales to any game where:


1. Moves are small (<1KB)
2. Move frequency is low (<10/sec)
3. Clients can validate moves independently


## What About Cheating?


This architecture trusts clients. Player A could modify their JavaScript to:


- Claim every attack is a hit
- See Player B’s ship positions
- Attack multiple times in a row


For a casual game, this approach may be acceptable. For competitive play, you need a server referee:


```text
// Pseudocode for server-validated moves
async     function     attackCell  (  row, col  )   {
const   response =   await   fetch(  "/api/validate-move"  , {
method  :   "POST"  ,
body  :   JSON  .stringify({ gameId, playerId, row, col })
});


const   { valid, result } =   await   response.json();


if   (valid) {
// Broadcast move via Agora
client.sendStreamMessage(  JSON  .stringify({
type  :   "attack"  ,
row,
col,
serverSignature  : result.signature   // Proof server validated this
}));
}
}
```


The server acts as source of truth, clients just render the results. Agora still handles real-time communication, but game logic moves server-side.


## Deployment Checklist


Before going live, verify:


1. Agora App ID: Replace the AGORA_APP_ID variable with your own
2. Token authentication: Current code uses null token (development only)
3. Lambda endpoints: Update agent function URLs
4. CORS headers: Ensure Lambda functions allow your domain
5. Rate limiting: Prevent message spam (though Agora has built-in limits)
6. Error boundaries: Add try-catch around all Agora calls
7. Logging: Implement analytics for game events
8. Mobile support: Test touch controls and mobile browsers


## Token Authentication


For production, generate tokens server-side:


```text
// Backend endpoint
app.post(  "/api/get-token"  ,   (  req, res  ) =>   {
const   { channelName, uid } = req.body;
const   token = RtcTokenBuilder.buildTokenWithUid(
AGORA_APP_ID,
AGORA_APP_CERTIFICATE,
channelName,
uid,
RtcRole.PUBLISHER,
Math  .floor(  Date  .now() /   1000  ) +   3600     // 1 hour expiry
);
res.json({ token });
});
```


```text
// Frontend
const   response =   await   fetch(  "/api/get-token"  , {
method  :   "POST"  ,
body  :   JSON  .stringify({ channelName, uid })
});
const   { token } =   await   response.json();
await   client.join(AGORA_APP_ID, channelName, token, uid);
```


Never expose your App Certificate in client code.


## Common Pitfalls


## 1. Forgetting to Unsubscribe


When cleaning up, stop all tracks:


```text
async     function     leaveChannel  (  )   {
if   (localAudioTrack) {
localAudioTrack.close();
localAudioTrack =   null  ;
}
if   (localVideoTrack) {
localVideoTrack.close();
localVideoTrack =   null  ;
}
if   (remoteAudioTrack) {
remoteAudioTrack.stop();
remoteAudioTrack =   null  ;
}


await   client.leave();
client =   null  ;
}
```


Forgetting this causes memory leaks and keeps your camera/mic active.


## 2. Not Handling User-Published Events


If you don’t subscribe to remote users, you won’t see their video:


```text
client.on(  "user-published"  ,   async   (user, mediaType) => {
await   client.subscribe(user, mediaType);


if   (mediaType ===   "video"  ) {
user.videoTrack.play(  "remoteVideo"  );
}   else     if   (mediaType ===   "audio"  ) {
user.audioTrack.play();
}
});
```


This is easy to forget when testing with one browser. Always test with two separate browsers or devices.


## 3. Sending Messages Before Joining


```text
// Wrong: client not joined yet
client = AgoraRTC.createClient({...});
client.sendStreamMessage(  "hello"  );   // Error!
```


```text
// Right: wait for join
client = AgoraRTC.createClient({...});
await   client.join(...);
client.sendStreamMessage(  "hello"  );   // Works
```


Always` await client.join()` before sending messages.


## 4. Parsing Messages Without Try-Catch


```text
// Dangerous
function     handleStreamMessage  (  uid, msgData  )   {
const   msg =   JSON  .parse(  new   TextDecoder().decode(msgData));
// What if msgData is corrupted?
}


// Safe
function     handleStreamMessage  (  uid, msgData  )   {
try   {
const   msgStr =   new   TextDecoder().decode(msgData);
const   msg =   JSON  .parse(msgStr);
// Process msg
}   catch   (e) {
console  .error(  "Invalid message:"  , e);
}
}
```


Always validate message structure before using it.


## Conclusion: When to Use This Pattern


Use Agora’s data messaging for games when:


- ✅ You already need video/audio communication
- ✅ Moves are infrequent (<10/sec per player)
- ✅ Players are in the same channel anyway
- ✅ You want to minimize backend infrastructure


Don’t use it when:


- ❌ You need guaranteed message delivery with ACKs
- ❌ Game state is very large (>10KB per update)
- ❌ You require server-authoritative validation
- ❌ Moves happen faster than network latency allows


This Battleship implementation shows how real-time communication, lightweight state synchronization, and voice interaction can work together in a single system. The same approach can be applied to other turn-based use cases — such as chess, card games, or trivia — where players need both shared state and live interaction.


More importantly, it demonstrates that for low-frequency, non-critical updates, an existing RTC connection can be extended to handle both communication and basic state synchronization. This allows you to build on top of a single real-time layer, reducing architectural complexity and avoiding the need for additional services in early-stage or experimental implementations.


The real win? You build one thing — a video call — and get multiplayer game sync for free.


## Additional Resources


- [Agora Web SDK Documentation](https://docs.agora.io/en/video-calling/get-started/get-started-sdk)
- [Data Messaging API Reference](https://api-ref.agora.io/en/video-sdk/web/4.x/interfaces/iagorartcclient.html#sendstreammessage)
- [Agora Token Authentication](https://docs.agora.io/en/video-calling/get-started/authentication-workflow)
- [Building Voice Agents with Agora](https://docs.agora.io/en/voice-ai/)


## Sample Code Repository


Full source code for this Battleship game:[BattleshipAgora](https://github.com/AgoraIO-Community/BattleshipAgora)


‍
