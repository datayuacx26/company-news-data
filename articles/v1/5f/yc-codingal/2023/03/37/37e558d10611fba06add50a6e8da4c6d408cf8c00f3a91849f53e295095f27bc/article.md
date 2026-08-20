---
schema_version: "1.0.0"
document_id: "37e558d10611fba06add50a6e8da4c6d408cf8c00f3a91849f53e295095f27bc"
company_key: "yc-codingal"
company: "Codingal"
source_id: "yc-codingal-news-import-ee841b07dc2a"
canonical_url: "https://www.codingal.com/coding-for-kids/blog/rock-paper-scissors-game-in-python/"
published_at: "2023-03-18T05:37:10+00:00"
first_seen_at: "2026-07-23T06:01:58.538210+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:cf17ee6ba1888995c65bd54bf602be056ea6606eda804608d35b20b88ba20fd1"
---

# Rock, paper, scissors game in Python | Codingal

[Codingal](https://www.codingal.com/) >


[Coding for kids](https://www.codingal.com/coding-for-kids) >


[Blogs](https://www.codingal.com/coding-for-kids/blog) >


Guide to Create a rock, paper, scissors game using Python


# Guide to Create a rock, paper, scissors game using Python


Updated on October 15, 2025


Table of Contents


Introduction
What is Python
What is rock, paper and scissors
Step 1: Define the rules of the game
Step 2: Writing the code
Step 3: Understanding the code
Step 4: Running the code
Conclusion


##


Learn how to create a fun and engaging rock, paper, scissors game in Python with this step-by-step guide. Understand the rules of the game, and run it using a compiler.


##


Introduction


### ****


**What is Python?**


Python is a high-level programming language which makes the code very easy to read and understand. With no need for declaration of variables, parameters and functions, python’s codes are usually shorter compared to other programming languages. Python is a great language for beginners as it has a simple syntax and is easy to read.


## **What is rock, paper and scissors?**


Rock, paper, scissors is a game played by using hands which can also be used as a toss when a decision on something between two or more people cannot be made. To play this game, participants say “rock, paper, scissors” and then put their hands forward forming the shape of either of the three. Making a fist represents rock, an open hand with palm facing down represents paper and two fingers (index and middle finger) extended represents scissors. In this article, we will walk through how to make a rock paper scissor game in Python.


## Steps to Create Rock Paper and Scissor Game in Python


Here is the step by step guide to create Rock Paper and Scissor game in Python


###


Step 1: Define the rules of the game


In order to create a rock paper scissor game, we should be fully familiar with the rules of the game. The rules state that rock smashes scissors, scissors cuts paper, and paper covers rock. So, rock wins over scissors, scissors win over paper and paper wins over rock. With the understanding of the rules, we can proceed to create the game.


###


Step 2: Writing the code


To write the code for our rock paper scissor game, we’ll use a python language compiler. Here’s an example code for a simple rock paper scissor game:


```text
python


import random


options = ["Rock", "Paper", "Scissors"]


user_choice = input("Choose Rock, Paper, or Scissors: ")


computer_choice = random.choice(options)


print("You chose: ", user_choice)


print("Computer chose: ", computer_choice)


if user_choice == computer_choice:


print("It's a tie!")


elif user_choice == "Rock" and computer_choice == "Scissors":


print("You win!")


elif user_choice == "Paper" and computer_choice == "Rock":


print("You win!")


elif user_choice == "Scissors" and computer_choice == "Paper":


print("You win!")


else:


print("Computer wins!")
```


##


Step 3: Understanding the code


Let’s break down the code line by line and understand what actually happened. We start by importing the random module. This module will generate a random number or choice (if mentioned by the coder). In our code, this module will generate a random choice for the computer amongst rock, paper or scissors.
We have then defined a list of options (Rock, Paper, Scissors) that the user has to choose one from.
Next, we ask the user to input their choice using the input() function. The input that the user enters is stored in the user_choice variable. The input of the user is case sensitive in this case and it will act abnormal if the input of the user does not match any of the options we have declared above.
We use the random.choice() function to generate a random choice from the options we have declared above for the computer. The choice of the computer is stored in the computer_choice variable.
We then print out the user’s choice and the computer’s choice using the print() function. So the user can see what they choose and what the computer chooses.
We use a series of if and elif statements to determine the winner of the game. If the user and the computer make the same choice, it’s a tie. If the user chooses Rock and the computer chooses Scissors, the user wins. Similarly, if the user chooses Paper and the computer chooses Rock, or if the user chooses Scissors and the computer chooses Paper, the user wins. For the user to win, one of these conditions must come true and if none of these conditions are true, this means that the computer has won.


##


Step 4: Running the code


To write and run python code, we need a compiler. Online compilers are also available to write and run a code. Just search “python compiler” and you will have many results. Here is an example of a compiler running the code we have written above. You should see a prompt asking you to input your choice. Once you’ve made your choice, the program will print out the computer’s choice and the winner of the game.


##


Conclusion


We have learned what a rock, paper and scissors game is and the rules to play this game. We also learnt how to create a simple rock paper scissor game in Python.


While this game in python may seem basic, it’s a great starting point for learning programming concepts like conditional statements, functions, and modules.


As you become more comfortable with Python, you can add more features to the game, such as keeping score, allowing inputs from the users that are not case sensitive and allowing the user to play multiple rounds.


With a little creativity, you can create a fun and engaging game that people will enjoy playing.


[Coding for kids](https://www.codingal.com/coding-for-kids/?utm_source=eu-blog-rps-in-python) has many beneficial advantages that develop cognitive abilities, enhance communication and entrepreneurship skills, and stimulate creativity. Kids can learn how to create different games using Python.


Codingal offers[Python for kids](https://www.codingal.com/courses/python-for-kids/?utm_source=eu-blog-rps-in-python) course to help your child enhance cognitive, logical, and computational skills. We provide 1:1 live interactive online coding classes with expert coding instructors for kids.


Along with lifetime access to course content and downloadable learning resources, to cater to every child’s need by providing a personalized journey.


[Try a free class](https://www.codingal.com/register/?utm_medium=blog-page-cta)


Share with your friends


### Learn more about coding for kids:


[Simulating Physics in Scratch Without Any Math Background Parul Jain on September 22, 2025 Introduction Let’s face it, physics can feel scary, especially when it’s loaded with complicated formulas and...](https://www.codingal.com/coding-for-kids/blog/physics-in-scratch-programming/)[Coding for Kids & Teens in Michigan Amit Dhanwani on August 31, 2024 Explore the best coding classes for kids & teens in Michigan and sign up for a free trial now. Start your child's coding...](https://www.codingal.com/coding-for-kids/blog/coding-for-kids-in-michigan/)[How Computer Virus Works? Sumit Singh on October 17, 2023 Discover the basics of how computer viruses work. Explore the world of computer viruses and safeguard your data....](https://www.codingal.com/coding-for-kids/blog/how-computer-virus-works/)[How to Create a Color Game in Python Amit Dhanwani on December 10, 2023 Create a vibrant color game with Tkinter in Python. Join Codingal for hands-on Python courses. Start coding, start playi...](https://www.codingal.com/coding-for-kids/blog/create-a-color-game-in-python/)[MIT App Inventor Tutorial for Beginners: Build Your First Android App Divya Pandey on March 20, 2026 If you have ever wanted to build your own Android app but thought it was too difficult, this App Inventor...](https://www.codingal.com/coding-for-kids/blog/mit-app-inventor-tutorial-for-beginners-build-your-first-android-app/)[6 Amazing Tips to Teach Coding to Kids Rajnish Kumar on June 10, 2021 Introduction Many people think that parents need to possess some specialized coding knowledge to introduce their...](https://www.codingal.com/coding-for-kids/blog/6-amazing-tips-to-teach-coding-to-your-children/)
