---
schema_version: "1.0.0"
document_id: "c58dd2a5d66d6ebe31997230cf7a61f7886b76f9d7003c7b0c3f9bcdc614d0e6"
company_key: "unity-software-inc-common-stock"
company: "Unity Software Inc."
source_id: "unity-software-inc-common-stock-rss-726793b11211"
canonical_url: "https://unity.com/blog/interactive-writing-challenges-for-nonlinear-rpg-design"
published_at: "2026-03-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:37.442468+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:200fccb53a6cf8b398e33e4f460042583f16f47c99e61840df93719b05415e2b"
---

# The challenge of interactive writing: Designing a nonlinear RPG like Esoteric Ebb

**Esoteric Ebb *is taking Steam by storm – but how does a branching narrative CRPG like this come together? In this guest blog, Christoffer Bodegård unpacks complex narrative design decisions and his eight-year journey learning how to manage the creation of a truly nonlinear, dialog-based story.***


Ever since the first public playtest of *Esoteric Ebb* back in 2023, I have been asked one simple question over and over again: “ *How do you keep track of everything* ?” It’s a good question – a difficult one. Most likely, I have answered it in several different ways over the years, but never in a really satisfying way.… So let’s give it a try.


## (But first, a little background on *Esoteric Ebb)*


*Esoteric Ebb* is a massive, narrative-focused, nonlinear CRPG. No traditional combat, almost entirely dialog-based, a playtime anywhere between 45 minutes and 50 hours, and so, so many choices… Actual choices, the type that actively changes your experience in fundamental ways all the way up to credits. Yes, it’s *one of those* games. And it took a while to make: about eight years from start to finish, half of which was spent doing nothing, sitting alone in a room, learning how to do *interactive writing* .


This content is hosted by a third party provider that does not allow video views without acceptance of Targeting Cookies. Please set your cookie preferences for Targeting Cookies to yes if you wish to view videos from these providers.


## The three constraints of interactive writing


I define interactive writing within three somewhat arbitrary constraints/goals. These being:


- A high choice-to-text ratio
- >50% of content being dynamic
- An open-ended design


Take or leave them as you wish, but if you hit these three, then you’ve got a recipe for intense (potential) positive player agency. But immediately by looking at this you might run into a few thousand problems.


Let’s go over them.


```text
The pale man holds incredible power,   if   he   is   able to bypass the esoteric limitation.   #int #DC10
"However, I suggest you speak with Lady Sageleaf about this, if you require answers. I am simply a middle-man in this."     #Visken
-(riverHub)


+.Snell==  1  -  "Snell."   \(Look at him.\)
The goblin gives you a conflicted expression.   #Snell
"I... didn't know, Cleric. I really didn't."     #Snell
"He did not."     #Visken
"Lady Sageleaf instructed me to not inform anyone. Even her own agents."     #Visken
"No need to be paranoid."     #Visken


++  "How can I not be paranoid? Everyone's lying to me!"
"Correction: I was lying to you. Lady Sageleaf, potentially as well."     #Visken


++  "True. It's just some esoteric bullshit."
"...Indeed."     #Visken
--
Snell scratches his chin.   #Snell
"I'm not entirely sure how to feel about this."     #Snell
"The Old Lady isn't one to spend money on resurrecting a city cleric."     #Snell
"But I'm guessing she didn't want to wait for another one to appear?"     #Snell
"Hm. We should go speak with her. But... remember that she paid for your resurrection, Cleric."     #Snell


++  "That is true. She seemed nice enough."
"...I wouldn't use 'nice'. But she's at least very, very competent."     #Snell


++  "I'll refrain from commenting until I hear her side of it."
"Smart."     #Snell


++  "I'm not sure I believe you."
"That's okay."     #Snell
"But really, I'm not sure what I would gain by lying about this."     #Snell
--
The goblin   is   telling the truth. You can read it   in   his eyes.   #wis #DC14
It was a LIE. But it was a LIE   for   <b>expediency</b>...   #con #DC9


+  "What spell did you use to revive me?"
He   is   quiet   for   several seconds.   #Visken
Considering   if   he should share   this   knowledge.   #wis #DC13
A modification, then.   #int #DC12
A <i>personal</i> resurrection spell.   #reply #int
"<i>Raise Dead</i>."     #Visken
"With changes. It is not the original spell, as I'm sure you understand."     #Visken
<i>Visken  's Raise Dead</i>. #int #DC12
"My spell requires a higher cost, while lowering the spell'  s complexity   in     return  .  " #Visken


++"  A higher cost? Like... a blood-cost?  "
"  No, Cleric.  " #Visken


++"  What   do   I owe you?  "
"  Nothing. Lady Sageleaf offered to pay it   in   full.  " #Visken


++"  A higher crown cost?  "
"  Yes, Cleric.  " #Visken
--
"  The   new   cost   is   doubled. One thousand crowns.  " #Visken
"  Which   is     not   too bad.  " #Visken


++"  My life   is   worth much more than that, yes.  "
++"  Eh. Could be better.  "
++"  Waste of money.  "
--
The pale man stares at you. #Visken


+"  Is   this   why I lost my ability to cast higher level spells?  "
"  Potentially.  " #Visken
"  Whatever issues your body   and   soul appeared to have had   with   my spell, it was   not   permanent.  " #Visken


++"  Yeah, I  've rebounded quickly."
"Yes indeed." #Visken


++"I'  m just that awesome of a wizard, I guess.  " #.WIZARD+=1
"  Of course.  " #Visken


++"  My soul <i>  and  </i> body?  "
"  It  's more common than you might think." #Visken


+"I felt really weird when I woke up. Like... different. Is that your dark magic also?"
"That can happen, yes." #Visken
"As it can with any resurrection magic when a soul is given sufficient time spent away from its body." #Visken


++"So my soul is ruined?"
"No." #Visken
++"I like this new me."
"Wonderful to hear." #Visken
++"I don'  t think the difference was that big, honestly.  "
"  Good.  " #Visken
++"  Can I go back to my old self?  "
"  No.  " #Visken
--
His cold voice shows no interest in your personal business. #wis #DC10
Eh. Whatever changed, it was probably for the best. #dex #DC16
Still got your wits about you. That's what matters. #reply #dex
At least you are still highly intelligent. #int #FC8
That is all that matters. #reply #int
Soul magic is complicated. Not even the greatest Arcanists were able to unlock all of the secrets of the <i>inner light</i>. #int #DC15
Perhaps one day, you will. #reply #int


+"  Right. I  'll... go speak with Lady Sageleaf."
"Good idea." #Visken
The pale man was simply an instrument. #int #DC11
But it is good to understand this game you'  ve been entangled within.   #reply #int
->questions


-
->riverHub
```


## Optimizing player engagement through the choice-to-text ratio


### Why high choice-to-text ratio matters


A high choice-to-text ratio sounds lovely. It’s a great tool to keep the player awake and engaged, for one. If you force them to read too many blocks of text, then the average player’s eyes will just glaze over. But if you keep poking and prodding at them – forcing them to answer interesting questions, pushing them into difficult decisions, or just forcing them to respond to rude accusations – then it’s like injecting an energy drink into their veins. They’ll jolt awake, and actually engage with the text. At least, statistically they’ll be more likely to do it.


The problem here is simple, though: You’d have to be *insane* to want to add that level of branching to your dialog. It takes a lot of work, and I think more specifically, it requires a very specific toolset. (Skills too, sure, but the tool has to be flexible and quick in order for the process of branching to not turn into an endurance test).


Esoteric Ebb | Christoffer Bodegård


### Choosing the right tool


The tool I use is the magnificent and incredibly wonderful[ink script](https://assetstore.unity.com/packages/tools/integration/ink-integration-for-unity-60055?srsltid=AfmBOore0z8uhu_I8GE1WU8-rK6oz4NiyJUUQVQe84d8K3hz2op-RmiV) created by the studio Inkle. By making this open-source tool – that works flawlessly in Unity, by the way – they have quite literally made my career. I owe them a lot. And ink is a tool that, apart from all the other cool stuff it does, is extremely well-suited for fast and nimble branching.


When I write a dialog in ink, I write with the same speed as with linear content. As long as there is a loose design outline prepared, I can just go to town. Adding player expressions, managing visuals, or handling dice checks – all take seconds, since everything is handled through basic (mostly customized-to-fit- *Ebb* ) tags and custom code, alongside the regular (and well-designed) ink functions.


```text
"Beyond that, I also focus a lot on my own unverified theories..."     #Snurre
He slaps his knees   and   grins.   #Snurre
"<b>The Folk Spirit</b>. Have you heard of it?"     #Snurre
It  's something he made up. Doesn'  t mean it  's not correct though. #int #DC18
Sounds like a collective folk view on morality. #wis #DC13


+"No. I have not."
--(spiritNo)
"Understandable. It'  s   not   yet published.  " #Snurre
+DC13 wis-"  Is that some kind of unified moral   or   ethical code of a people   or   culture?  "
"  No, it  's-" #Snurre
He leans back and squints. #Snurre
"That'  s   not   actually that far off. You  've got a head on you, Cleric." #Snurre
+"Yes."
"Oh!" #Snurre
"Then tell me, what is it?" #Snurre


++"I lied. I have no idea what you'  re talking about.  "
->spiritNo


++ROLL18 int-Figure it out.
+++S
Ah... #int
Just look at this halfling. #int
He's clearly walking in the footsteps of the study of unconscious mind melding. #int
Your best guess would then be... #int


++++"  The Folk Spirit...   is   that based   on   the idea of universal folk myths?  "
His bushy eyebrows reach for the ceiling. #Snurre
"  Why, yes! Very good guess, Cleric.  " #Snurre


+++F
You stare at the halfling for about thirty seconds. #int
"  You  're a quiet one, Cleric." #Snurre
An idea pops into your empty, empty skull. #int


++++"The Folk Spirit is a communal ghost that haunts us every autumn."
++++"The Folk Spirit is a great Fordnippian wine."
++++"The Folk Spirit? That'  s a music genre.  "
----
"  ...Not a terrible guess, but no.  " #Snurre


++++"  Yeah, I have no idea.  "
->spiritNo


-
"  The Folk Spirit   is   the working title   for   my   new   thesis.  " #Snurre
"  Most of it   is  ,   if   you excuse my anuran, <i>fucking bullshit</i>.  " #Snurre #XPGain #Minor
"  But   in     short  , it   is   about how each folk share a number of core communal archetypes,   or   myths. I have traveled around the Coast to collect   as   many tales, writing down thousands of stories previously only passed down orally.  " #Snurre
"  All to see how well my theory holds up,   in   the minds of so, so many folk.  " #Snurre
He clears his throat and leans back into his chair. #Snurre
"  Apologies. I am ranting here,   let   us put a stop to it. What   do   <i>you</i> want, Cleric?  " #Snurre
->hub
```


## Planning for variability: Dynamic content and open-ended design


But what about item number two of our list of constraints (“>50% of content being dynamic”)? Dynamic content and open-ended (or nonlinear) designs both stem from a similar goal: to create variability based on player input.


Just let them do whatever they want! Go nuts! Have an open world, where you can go in any direction! Well, that’s one way to do it. But the important distinction here is that interactive writing always focuses on an authorial-intent-driven design. In other words, everything is controlled by the writer.


You can still do systematic design, and there’s a big overlap, but the *practical art* of interactive writing is specifically when you *do not let* emergent storytelling do its thing.


What I mean by this is: You have to have variables. And you have to keep track of them. For every choice the player makes, whether it’s stats chosen during character creation, or how mean they were against that goblin chieftain – all of it needs some type of feedback in order to invoke the phenomenon of agency. Whether that involves a dynamic quip towards the end of the game, or an entire branching plotline, those moments of feedback are just as important as the choices themselves, if not more.


Esoteric Ebb | Christoffer Bodegård


### Keeping track of variables


My method of tracking variables is simple and flexible: I call it the Story Variable (SV) system. Using the tag system, whenever those tags begin with a punctuation, that indicates the usage of a variable. If that variable has never been encountered before, the SV is created in one giant list. Otherwise, it just accesses the already-existing SV, and either sets or checks it as commanded.


```text
++DC20 wis-\(Look   for   the source of the breeze.\)
Your eyes glance upwards.   #wis
The wall. The wall   is     not   a wall.   #wis #.TE_SecretDoorRevealed=1 #UpdateEntities
Look at the wall.   #wis
+++E-Oh?
->END
```


A string and an int – usually used as a boolean, but when needed expands to increase or decrease whenever needed. The commands I implemented for Ebb were “==”, “=”, “>=”, “<=”, “+=”, and “-=”.


The question then becomes one of organization. I got better at this as I went along, but each SV uses a prefix based on the location or quest. “TE” in this case refers to the Tea Shop area. An SV with the “Q” prefix refers to a quest, and a “QP” refers to a Quest Point –as in, a log entry in the Questing Tree, your quest journal.


```text
Yes.   #wis
A   symbol.   #wis     #XPGain     #Minor
A   symbol of   a   sun. Definitely.   #OBJ
If this is what Akzel wanted you   to   find, you've found it.   #wis   #  .QP3_Mine  =  1   #  .Q_Mine  =  2


+Huh. So what do   I   do now?
-
Get back   to   your dwarf buddy, of course!   #dex
```


### Narrative design gains


It’s a rough system, with one major productivity gain: You can just keep writing. Need a new variable for a dynamic dialog choice? Just add it, then copy-paste it into the file where it needs to be set. Easily check for any variable usage via a project-wide CTRL+F, or manage sweeping changes via basic text management. There’s no database to manage. The list itself is just chronological. Forgot the name of a variable? Just search for the relevant area or quest prefixes, and look over the list.


I did not expect this to work at first. An average playthrough can end up with more than 3,000 Story Variables in that list by the end of the game. But – like with most things I did on *Esoteric Ebb* – Unity just worked with it. And ink’s Unity integration has never failed me, not once, even after using it for almost a decade now. A recompile of the ink files takes seconds. Customizing the features to *Esoteric Ebb* ’s liking has always been extremely efficient. And as strange as it might sound, I’m still surprised all these years later that it *just flows* . If I am able, I want to keep developing this pipeline for years to come.


Beyond that, I use Notepad++ for all my writing on *Esoteric Ebb* . While you could obviously use any text editor of your preference, keeping it lightweight and snappy makes for such a smoother experience. Searching through a million words in a split second is exactly what allowed me to write (and to bugfix!) such a massive game.


```text
The croco-beast rushes forward   and   grabs you   by   the neck.   #Kraaid #HPLoss #1d4


++\(Struggle to breathe.\)   "I'm actually..."
He tightens the grip   as   you mutter   out   a faint response.   #Kraaid
+++  "...a rogue."     #.ROGUE+=1
+++  "...a wizard."     #.WIZARD+=1
+++  "...no, no okay, I'm a cleric. I'm <b>The Cleric</b>, even."     #.CLERIC+=1
+++.BARD==  1  -  "...I think I'm a Bard? You know what that is?"     #.BARD_Choice+=1
+++.DRUID==  1  -  "...a Druid. I guess?"     #.DRUID_Choice+=1
+++DC17 con-  "...<i>I'm Agrarian</i>...! No wait, shit. That's politics. I mean, uh..."     #.AGRARIAN+=1
+++  "...I'm whatever you want me to be. <i>Please don't kill me</i>."
"Whatever <wiggle>Kraaid</wiggle> wants?"     #Kraaid
++++  "Yes...! I'll even be apolitical!"     #.APOLITICAL+=1
++++  "Yes! I'm... I'm a rabbit!"


++  "No! I am! I'm Cleric! That's me!"
```


## Final thoughts: Bugfixing and learning to embrace the branch


But that’s also the biggest negative of this system: the bugfixing. I should have invested more time in technical solutions to broken logic and syntax, but really, I ended up just brute-forcing the whole ordeal via playtesting.


As *Esoteric Ebb* receives its 1.1 patch, I have managed to fix approximately 704 ‘text errors’ – whether spelling or code related – in less than four days of work (very chill days, too). This is, again, because the system is extremely nimble. Even still, those bugs were there when we launched *Esoteric Ebb* because of the setup here as well. It is an insane amount of text, with a ridiculous amount of branching. But I am also certain that I would not have been able to write even a quarter of the final word count had I not been blessed with ink.


Esoteric Ebb | Christoffer Bodegård


## Afterword: More recommended tools for narrative designers


ink makes the practical art of interactive writing a fast one, for *me* . I’ve always had trouble with visual scripting, but I know many writers have the exact opposite problem. So I always recommend looking into other solutions as well to find the one that fits you and your team the best, with some examples being[articy:draft](https://assetstore.unity.com/packages/tools/integration/articy-draft-3-importer-71191) ,[Arcweave](https://assetstore.unity.com/packages/tools/integration/arcweave-for-unity-148326?srsltid=AfmBOoogN5XSRe3U2xIf9e8NTlJEIehsbdZkuWRHCka4YZuCEDouN-ka) , and[Yarn Spinner](https://assetstore.unity.com/packages/tools/behavior-ai/yarn-spinner-for-unity-the-friendly-dialogue-and-narrative-tool-267061?srsltid=AfmBOorfDahfXztCfbYIBxl1gHVID93gxy-nHBeXhbUzb4bZuVaNZ5KN) .


**[Esoteric Ebb](https://store.steampowered.com/app/2057760/Esoteric_Ebb/) *is available now on Steam. Explore more Made with Unity games on our[Steam Curator page](https://store.steampowered.com/curator/45049063-Made-With-Unity-Official/) , and check out more stories from Unity developers on the[Unity Blog](https://unity.com/blog) and[Resource hub](https://unity.com/resources) .***
