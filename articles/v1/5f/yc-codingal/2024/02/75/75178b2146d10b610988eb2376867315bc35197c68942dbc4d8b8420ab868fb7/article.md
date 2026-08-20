---
schema_version: "1.0.0"
document_id: "75178b2146d10b610988eb2376867315bc35197c68942dbc4d8b8420ab868fb7"
company_key: "yc-codingal"
company: "Codingal"
source_id: "yc-codingal-news-import-ee841b07dc2a"
canonical_url: "https://www.codingal.com/coding-for-kids/blog/how-to-make-flappy-bird-on-scratch/"
published_at: "2024-02-17T11:28:05+00:00"
first_seen_at: "2026-07-23T06:01:58.538210+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:435db95a4ff0ca8ee33ac09fa93976657af52dda553f6ae01843450d2d2be8a0"
---

# How to make Flappy Bird on Scratch in 8 easy steps

[Codingal](https://www.codingal.com/) >


[Coding for kids](https://www.codingal.com/coding-for-kids) >


[Blogs](https://www.codingal.com/coding-for-kids/blog) >


How to make Flappy Bird on Scratch in 8 easy steps


# How to make Flappy Bird on Scratch in 8 easy steps


Updated on November 29, 2025


Create the famous Flappy Bird game using Scratch coding in 8 simple steps. Learn to add obstacles, collisions, and score cards. Unlock a free scratch coding class and easy guides to creating other awesome games like Geometry Dash and Pac Man!


Table of Contents


Introduction
What is Flappy Bird?
Scratch Programming
How to make Flappy Bird on Scratch
Conclusion


##


## Introduction


Are you ready to become a game developer? In this tutorial, we will learn how to make Flappy Bird on[Scratch coding](https://www.codingal.com/coding-for-kids/blog/introduction-to-scratch-coding/) , a fun and interactive programming language.


With[Scratch coding](https://www.codingal.com/coding-for-kids/blog/introduction-to-scratch-coding/) , you can create your games and animations and even customize already existing games!


This guide will give you all the steps and tricks to conquer the world of Flappy Bird! So let’s get going!


## What is Flappy Bird?


Imagine just a simple mobile game that took the world by storm, becoming an overnight sensation that garnered 90+ million downloads. That is the story of Flappy Bird.


The game was developed by Dong Nguyen, a game developer in Vietnam, in approximately 2-3 days. Surprising, right?


The main character ‘Faby’, the bird, has to fly without being hit with the pipes. In this game, Faby automatically descends and only ascends when you tap on the touchscreen/space bar on the keyboard.


This arcade game has a straightforward interface. The game’s pipes were suspended at varying heights with equally sized gaps between them, like this:


Not only this, but the game was also easy to play. Each successful passage through the pipes earned you a point. But any collision with the bird means the game ends.


The most interesting fact about this game is that after nine days of release, Nguyen took Flappy Birds down from all platforms because it was too addictive and had compulsive effects on some people!


Now, different versions of this game exist, and today, you are going to make your version using Scratch coding!


## Scratch Programming


Scratch programming is a visual programming language and an online community developed by the MIT Media Lab. It is designed to teach coding concepts to beginners, especially kids, in a fun and interactive way.


Scratch coding provides a graphical interface where users can drag and drop blocks of code to create animations, games, stories, and interactive projects. If you are new to scratch programming, then we recommend you read the[A to Z Guide on Block Coding](https://www.codingal.com/coding-for-kids/coding-guides/block-coding-guide/) .


## How to make Flappy Bird on Scratch?


To create this iconic game, we’ll use the[Scratch editor](https://scratch.mit.edu/) . Before we jump into creating, let’s plan our game.


Think about what could make this game stand out from the original version. Think about the main character. Do you still want it to be faby? Do you want to add another animal instead of a bird? What do you want the backdrop to be?


Decide on the main theme and elements you want to include in your game.


### Step 1: Select the backdrop


Every great thumbnail needs a fantastic background. In Scratch coding, we can use the Stage as our canvas. Click on the “Stage” icon at the top to open the Stage editor. Choose a backdrop that matches your project’s theme. It could be a starry night, a beautiful landscape, or anything related to your idea!


As we are going with the traditional Flappy Bird game, we will go for a solid blue background.


### Step 2: Adding characters


Now, let’s bring our fabby to life! You can create your[sprites](https://www.codingal.com/coding-for-kids/blog/what-is-sprite-costume-in-scratch-programming-and-how-to-use-it/) (characters) from Scratch or choose from Scratch’s vast library of sprites.


Click[here](https://www.codingal.com/resources/wp-content/uploads/2024/02/bird.png) to download Fabby on your desktop.


Go to the “Choose a Sprite” button and click on “Upload a Sprite”. Now select the bird.png image from your desktop.


Now, this is what my console will look like:


### Step 3: Sprite Code


Now, this is where the magic happens. We want that whenever we start the game; the bird keeps falling down until and unless the player presses the space bar.


It’s just like how gravity will make anything fall down from the sky. To apply this gravity effect on your Sprite, make the following code block:


Whenever we click on the “Green Flag,” the bird goes down.


We are done with one part of the logic. The other half states that whenever you press the spacebar, the bird should fly up.


Now, let’s add this code snippet to your sprite:


Together, our two code blocks will make the bird fly:


Kudos for getting this far! Let’s add some obstacles now!


### Step 4: Adding Obstacles


Let’s add some pillars, or whatever obstacle idea you have in your mind. To download the pillar image, click[here](https://www.codingal.com/resources/wp-content/uploads/2024/02/pipe.png) .


Just like you added the bird to your project, add this pillar too.


Now, click on the costume for this pipe, and make a duplicate. You can do this by clicking on the pipe, and pressing “Ctrl + c” and then “Ctrl + v”.


Then, press the “Flip Vertical” button as shown and arrange the pillars in this manner:


The next step is to make the backdrop move so that our game can proceed!


### Step 5: Obstacle Code


You’ve worked hard on your game so far! Congratulations!


Now, let’s code some more. Click on the pipe sprite and add this code block. What this code block will do is add more pipes when the bird flies to the right.


### Step 6: Game Over Screen


Now, if the player cannot win your exciting game, you need to end the game and display a new backdrop.


Make a new sprite, click on the costume button and add a simple backdrop. This is similar to what you did above to add the new backdrop. Let’s add this text now:


We just want this “Game Over” backdrop to show up when the players lose the game. To achieve this, add this code block:


### Step 7: Obstacle Collision


We want that whenever Faby touches any of the pipes, the game must end. For this, we need to add a conditional statement to the Faby Sprite.


You have already added a few code blocks for this Sprite; all you need to do is make a few changes to them:


The last step is to add a scorecard!


### Step 8: Score Card


Let’s make a new[variable](https://www.codingal.com/coding-for-kids/blog/variables-in-scratch/) “score:”


Now, we want the score to increase by one whenever Faby crosses a pillar successfully. But, if the sprite touches any of the pillars, we want the game to end.


Make the following changes in the code block of Faby:


Now, go to the code block of the pillar and add this code block:


### Step 9: Time to Play


Yay! You have just made your Flappy Bird Game and now you can enjoy it to the fullest! Make sure to challenge your friends and family members to play this game that you just created!


## Conclusion


Congrats! You’ve learned how to make the fan-favourite Flappy Bird game with your twist.


Remember, have fun experimenting, be bold with your ideas, and let your creativity soar as you create more and more magical games for your fantastic Scratch projects!


Scratch coding is a powerful tool that allows you to unleash your creativity and learn the basics of coding in a fun and engaging way. Begin your journey in scratch programming with a **[free scratch coding class for kids](https://www.codingal.com/lp/book-scratch-trial-class/?course=scratch-programming-trial-class&utm_source=exu_blog_page&utm_medium=organic)** . Explore the **[best scratch coding course for kids](https://www.codingal.com/courses/scratch-programming/?utm_source=exu_blog_page&utm_medium=organic)** and unlock your full potential. So keep learning with us, and who knows what amazing projects you’ll create next?


## Bonus Section


Explore our easy guides to building some of the most famous games on Scratch programming, add your twist to them by[adding sounds](https://www.codingal.com/coding-for-kids/blog/add-sound-effects-to-scratch-coding/) ,[multiple levels](https://www.codingal.com/coding-for-kids/blog/how-to-add-levels-in-a-game-on-scratch/) , and[scrolling backgrounds](https://www.codingal.com/coding-for-kids/blog/make-a-scrolling-background-in-scratch/) and share with your friends to play!


[How to make a Geometry Dash game?](https://www.codingal.com/coding-for-kids/blog/guide-to-making-a-geometry-dash-scratch-game-codingal/)
[How to create a Jumping game in Scratch?](https://www.codingal.com/coding-for-kids/blog/how-to-create-jumping-game-in-scratch/)
[How to create the classic Snake game using block coding?](https://www.codingal.com/coding-for-kids/blog/how-to-create-classic-snake-game-using-block-coding/)
[How to create a Pac-Man game using Scratch?](https://www.codingal.com/coding-for-kids/blog/how-to-create-pac-man-game-using-scratch/)
[How to create a fruit-ninja game on Scratch?](https://www.codingal.com/coding-for-kids/blog/how-to-create-a-fruit-ninja-game-on-scratch/)


Share with your friends


### Learn more about coding for kids:


[Why Learning HTML is Good for Your Child ? Sumit Singh on August 5, 2023 HTML is the building block for the web, and here's why HTML is an excellent skill for your young ones to pick up!...](https://www.codingal.com/coding-for-kids/blog/why-learning-html-is-good-for-your-child/)[What is Computational Thinking and Why is it Important for Kids? Parul Jain on October 9, 2025 In today’s rapidly evolving digital world, computational thinking (CT) is no longer just a buzzword—it’s a...](https://www.codingal.com/coding-for-kids/blog/computational-thinking/)[Coding for Kids & Teens in Orleans, LA Amit Dhanwani on August 31, 2024 Explore the best coding classes for kids & teens in Orleans and sign up for a free trial now. Start your child's coding ...](https://www.codingal.com/coding-for-kids/blog/coding-for-kids-in-orleans/)[What Kids Need to Know about Version Control Amit Dhanwani on May 2, 2023 In today's digital age, technology plays a significant role in our lives, and coding courses have become valuable for...](https://www.codingal.com/coding-for-kids/blog/what-kids-need-to-know-about-version-control/)[Roblox AI Coding: Code on Roblox using Generative AI Sumit Singh on February 10, 2024 Explore Roblox AI coding: Learn how to use Roblox AI for game development, make a Roblox game with AI, and master AI cod...](https://www.codingal.com/coding-for-kids/blog/roblox-ai-coding-code-on-roblox-using-generative-ai/)[Block Coding for Kids: How Coding Blocks Help Kids Learn Faster Divya Pandey on March 2, 2026 Block coding is the smartest, most beginner-friendly way to start programming - and in 2026, it is how...](https://www.codingal.com/coding-for-kids/blog/block-coding-for-kids-how-coding-blocks-help-kids-learn-faster/)
