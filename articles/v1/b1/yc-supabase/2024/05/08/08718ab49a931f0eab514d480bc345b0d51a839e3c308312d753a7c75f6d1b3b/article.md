---
schema_version: "1.0.0"
document_id: "08718ab49a931f0eab514d480bc345b0d51a839e3c308312d753a7c75f6d1b3b"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/meetup-kahoot-alternative"
published_at: "2024-05-09T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:00:17.354967+00:00"
content_hash: "sha256:771b68cc3446dd6e014a84bacc30fc8aa1ebc1e2d2923bbdeee7c121db1bb23a"
---

# The open source Kahoot alternative

Recently as part of our[GA Week](https://supabase.com/ga-week) and with the help of our amazing community leaders we had meetups at over 30 different locations worldwide! Seeing all the fun pictures and videos of the community meetups was amazing!


To make the meetups more engaging, we prepared an interactive quiz game that everyone could play. We were initially going to use Kahoot for this. Still, we thought it would be an excellent opportunity to showcase the power of Supabase and open source, so we built an open-source version of Kahoot with some pre-filled Supabase / tech questions for everyone to play at the meetup.


## What we built for the meetup#


The Kahoot alternative that we built is a simplified version of Kahoot. Mainly having fewer features on the admin side of things, but as an individual player participating, the experience should be similar to the actual Kahoot.


The game has two screens: the host screen and the player screen. The host can start a game by choosing one of the available quiz sets. Because we had meetups from around the globe, we created the same 10-question quiz sets in 4 different languages.


Once the host clicks the button to start a game, a lobby screen with a QR code appears. The players can scan the QR codes with their phones, name themselves, and enter the game.


The host can start the quiz game once everyone in the room has entered the lobby. The players are shown the question first and then the choices five seconds later for each question. They answer the quiz on their phones, and the sooner they answer, the more points they get.


Once all the questions are answered, the points are summed up, and the leaderboard is shown on the screen.


Actual leaderboard from the Tokyo meetup.


## Building the Kahoot alternative app#


I will not go into every implementation detail, as it would be long, but you can find the GitHub repository with the full code[here](https://github.com/supabase-community/kahoot-alternative) . The app is a Next.js app without using any server-side rendering, and it utilizes three core Supabase features: auth, database, and realtime.


### Auth#


For authentication, we used the brand new[anonymous sign-in](https://supabase.com/docs/guides/auth/auth-anonymous) feature to sign in the players automatically in the background. This way, players don’t feel the friction after scanning the QR code and can get started with the game quickly. It also allows us to set a row-level security policy to ensure players have limited read and write permissions in the game.


When a user registers for a game, we check if the user is already signed in, and if not, the app signs them in.


`
_10


const { data: sessionData, error: sessionError } = await supabase.auth.getSession()


_10


_10


let userId: string | null = null


_10


_10


if (sessionData.session) {


_10


userId = sessionData.session?.user.id ?? null


_10


} else {


_10


const { data, error } = await supabase.auth.signInAnonymously()


_10


userId = data?.user?.id ?? null


_10


}


`


### Database#


The database stores everything that happens in the app. We have 6 different tables to store everything from the questions to the answers that users leave for each question in a game. Each time the admin clicks the “Start Game” button of a quiz set from the host dashboard, a new game is created. The database is designed so that the same set of users can play games with the same quiz set multiple times. The full table definitions under` superbase/migrations` in the[GitHub repo](https://github.com/supabase-community/kahoot-alternative/tree/main/supabase/migrations) .


### Realtime#


Realtime is the core of the app. The critical feature of Kahoot is to keep all the connected clients in sync with the game state, so we heavily relied on realtime to share the game states. The admin subscribes to Postgres changes on the` participants` ,` games` , and` answers` table. The subscription on the` participants` table is used when it first displays the list of participants on the lobby screen. The subscription on the games table is used to listen to the game state as the admin clicks the next button to move through the questions. Subscriptions on the answers table count how many people have answered the question so that the results can be displayed when everyone has answered the question.


Within the host page, we chain multiple Postgres changes listeners like the following to bundle the realtime subscriptions into one.


`
_31


supabase


_31


.channel('game')


_31


.on(


_31


'postgres_changes',


_31


{


_31


event: 'INSERT',


_31


schema: 'public',


_31


table: 'participants',


_31


filter: \`game_id=eq.${gameId}\`,


_31


},


_31


(payload) => {


_31


setParticipants((currentParticipants) => {


_31


return \[...currentParticipants, payload.new as Participant\]


_31


})


_31


}


_31


)


_31


.on(


_31


'postgres_changes',


_31


{


_31


event: 'UPDATE',


_31


schema: 'public',


_31


table: 'games',


_31


filter: \`id=eq.${gameId}\`,


_31


},


_31


(payload) => {


_31


const game = payload.new as Game


_31


setCurrentQuestionSequence(game.current_question_sequence)


_31


setCurrentScreen(game.phase as AdminScreens)


_31


}


_31


)


_31


.subscribe()


`


## The Results#


Through this open-source app created using Next.js and Supabase, we brought everyone together during meetups worldwide by providing an engaging experience.


Thanks to everyone who participated. I hope you enjoyed it, and a massive thank you to everyone who hosted the meetups.


> I’m proud of my second place showing at the Salt Lake City[@supabase](https://twitter.com/supabase?ref_src=twsrc%5Etfw) meetup hosted by[@KyleRummens](https://twitter.com/KyleRummens?ref_src=twsrc%5Etfw) .[pic.twitter.com/Uzk4q3aJyy](https://t.co/Uzk4q3aJyy)
>
>
> — Tristan Rhodes (@tristanbob)
>
>
> [April 19, 2024](https://twitter.com/tristanbob/status/1781340495899406511?ref_src=twsrc%5Etfw)


> Yesterday's evening, I attended[@supabase](https://twitter.com/supabase?ref_src=twsrc%5Etfw) first Meetup in Nigeria. Learnt about Supabase and why I should consider using it.
> I also won a cash price for being the first position in the Kahoot game . Shout-out to the organizers[@GeekyAdams](https://twitter.com/GeekyAdams?ref_src=twsrc%5Etfw) .[pic.twitter.com/OtXUaXvB5o](https://t.co/OtXUaXvB5o)
>
>
> — Dherrbie (@Debbyiecodes)
>
>
> [April 20, 2024](https://twitter.com/Debbyiecodes/status/1781626949552525323?ref_src=twsrc%5Etfw)


## Contributing to open source#


Kahoot is a complex app with many features, and many of those features are missing from this alternative app. Especially on the host dashboard, there are a lot of things that could/ should be added, like adding/ editing quz sets or looking back on past game results. Again, the GitHub repo can be found[here](https://github.com/supabase-community/kahoot-alternative) , and we are always welcome to receive pull requests. If you want to get into Open source, a casual project like this is a great starting point. Any kind of PR is welcome. Adding/ editing docs/ readme is greatly appreciated, as well. If unsure where to start, ping us about the issue, and we can help you.


## Resources#


- [Kahoot alternative GitHub repo](https://github.com/supabase-community/kahoot-alternative)
- [Anonymous Sign-in guide](https://supabase.com/docs/guides/auth/auth-anonymous)
- [Realtime Postgres changes guide](https://supabase.com/docs/guides/realtime/postgres-changes)
- [Supabase Next.js quick starter guide](https://supabase.com/docs/guides/getting-started/quickstarts/nextjs)
