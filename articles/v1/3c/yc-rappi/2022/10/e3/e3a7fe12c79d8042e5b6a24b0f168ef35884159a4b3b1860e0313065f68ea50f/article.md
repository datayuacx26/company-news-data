---
schema_version: "1.0.0"
document_id: "e3a7fe12c79d8042e5b6a24b0f168ef35884159a4b3b1860e0313065f68ea50f"
company_key: "yc-rappi"
company: "Rappi"
source_id: "yc-rappi-rss-63ff898fda0d"
canonical_url: "https://engineering.rappi.com/clean-code-for-data-scientists-f15dab08483f"
published_at: "2022-10-03T22:39:22+00:00"
first_seen_at: "2026-07-20T23:20:59.100260+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:75ee0efcaf2b2ff66a0f2b2c48a4690b45d08ed1c6c25c44583e494f0ecd8b23"
---

# Clean code for Data Scientists

Press enter or click to view image in full size


# Clean Code for Data Scientists


[Salvador Barcenas Valladolid](https://medium.com/@salvadorbv?source=post_page---byline--f15dab08483f---------------------------------------)


5 min read


·


Oct 3, 2022


--


## Introduction


As we know there are many roles involved in the data field such as data engineers, data architects, machine learning engineers, data analysts, data scientists, etc. If you look at the things these roles share in common you could say: ¡Data!, and you are right. But there is another one that is more involved in some of them but at least everyone knows the concept: Coding.


Data scientists are highly educated. Their most common fields of study are Mathematics and Statistics (25%), followed by Computer Science (20%), Natural Sciences such as Physics (20%), and Engineering (18%)¹. Many of which lack exposure to the set of skills required for writing quality code. Don’t worry, we are constantly learning through our field, so ¿Why not take a look at software engineering best practices?³


Press enter or click to view image in full size


## Clean code tips⁴


### Use meaningful and pronounceable variable names


Sometimes we like to use shortcut names applied in statistics, machine learning, etc, but here we are assuming all people involved in the ML pipeline know about those, and that’s not necessarily true.


```text
gs = GridSearchCV(estimator=SVC(),param_grid=params,verbose=1)
```


Later, when we will use the GridSearchCV in the training step, we can get confused about what’s ‘ps’. It could be: a good score, but who knows? A better name would be:


```text
grid_search_cross_validation_model = GridSearchCV(estimator=SVC(),param_grid=params,verbose=1)
```


Yes, it’s better a large name variable than a nonsense one.


### *Limit the number of function parameters*


If you take a look at the number of parameters used in functions from the Scikit-Learn library you’ll see have a lot of parameters, but ¡hey! they are created to train models, preprocess data, and much more, it’s reasonable.


But for example, if you are creating a function that outputs your model’s metrics, it’s not a good idea to have a parameter for each metric like this:


In this case, if the function needs different paths, and you want to get rid of taking care of path standardization, you could create a class to manage it.


This way the functions are cleaner, and you don’t have to worry about many parameters. Let the class handle it. But that’s not the best part, if you work as a team, your colleagues will thank you.


### Functions should do one thing


It’s difficult to achieve this one as all can be decomposed, but once you understand how this work you will succeed in your ability to write clean code. This is one of the most important tips, as it’s simpler to test functions that do one thing.


Let’s illustrate with an example I downloaded looking at Kaggle competitions, in particular from[this](https://www.kaggle.com/code/sohileadel/titanic-classification-problem) Jupyter Notebook²:


As it might be seen, this function is performing several things:


1. Generating polynomial features
2. Training with GridSearchCV
3. Obtain the best parameter configuration model


Let’s use our last previous code tips to refactor this function and illustrate the concept:


Maybe there is a better way to refactor the previous function, but when you have done the refactor ask yourself the following questions:


1. Is the refactored function easier to understand?
2. Can I decompose the function into some others?


If one of the answers is ‘yes’ (it can be both), iterate the process.


## Get Salvador Barcenas Valladolid’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


Now you may be asking where are the ‘ *train_with_polynomial_features* ’ and ‘ *obtain_best_model_configuration* ’ functions and what’s utils?. The answer is in the following code:


You can forget about these functions since are being used by the main function.


### Don’t repeat yourself (DRY)


I have seen this one many times, especially when you are doing preprocessing or feature engineering to the train/test data. You can avoid duplicated code by creating classes or functions.


In this example maybe it’s not too obvious the duplicated code, but imagine if you are doing a lot of transformations and so on.


### Function names should say what they do


This is similar to variable names, but sometimes we need to think carefully to name some function, and that’s why it is important functions should do one thing.


I recommend these two things to consider when naming functions:


1. If you can’t discompose your function and you think it’s doing two things, think about what’s the reason you created it.


Ok, this function name is describing everything it does, but we need to abstract this and name it with its main purpose. A better name would be *train_with_polynomial_features* .


2. The second thing to consider is to be sure about what your function does. It can happen your function name is *train_model* but is getting the metrics of the model. Be aware of this because if you want to abstract your function you can make confusion about it.


### Use comments carefully


At the beginning of using a new set of libraries, we comment on some lines to remember the intent of some functionality, and it’s ok. But don’t abuse them. Here are some examples:


```text
# Here I'm filtering my training data with variable_2 equals 1  train = train[train["VARIABLE_2"] == 1]
```


As you can see, the comment is redundant.


```text
# Obtain the age of each person  def diff_dates(date1):      return abs(date2 - ymdstr).years  data["AGE"] = data["BIRTH_DATE"].apply(lambda x: diff_dates(x))
```


Maybe this one is useful, but if you use the above tips, like ‘ *Use meaningful and pronounceable variable names* ’, you can rid of some comments.


```text
def obtain_age(birth_date):      return abs(birth_date - current_date).years  data["AGE"] = data["BIRTH_DATE"].apply(lambda x: obtain_age(x))
```


### Don’t stop learning


This is the most important advice I can give you. It doesn’t matter if your code is very bad, you can use the above tips and explore the following resources to learn more about best software engineering practices. At the end of the day, our field is constantly bringing new knowledge, and I love it.


[Refactoring and Design Patterns Refactoring.Guru makes it easy for you to discover everything you need to know about refactoring, design patterns… refactoring.guru](https://refactoring.guru/?source=post_page-----f15dab08483f---------------------------------------)


[Design Patterns and Refactoring Unified Modeling Language makes it possible to describe systems with words and pictures. Especially notable use case… sourcemaking.com](https://sourcemaking.com/?source=post_page-----f15dab08483f---------------------------------------)


So, looking at the meme at the top we can reformulate the question and answer.


*¿Can data scientists write good quality code?*


Thanks for reading. We can connect on:


[Linkedin](https://www.linkedin.com/in/salvador-barcenas-valladolid-ab4940209/)


[Github](https://github.com/SalvadorBValladolid)


### References


\[1\]: Burtch Works. (November 17 2014). *The Must-Have Skills You Need to Become a Data Scientist*


[The Must-Have Skills You Need to Become a Data Scientist - Burtch Works Update 2018: Years after publication, this post continues to generate a lot of interest! As such, we've updated it to… www.burtchworks.com](https://www.burtchworks.com/2014/11/17/must-have-skills-to-become-a-data-scientist/?source=post_page-----f15dab08483f---------------------------------------)


\[2\]:[https://www.kaggle.com/code/sohileadel/titanic-classification-problem](https://www.kaggle.com/code/sohileadel/titanic-classification-problem)


\[3\]: Robert C. Martin (2008);Clean Code: A Handbook of Agile Software Craftsmanship


\[4\]:[Robert C. Martin (2008);Clean Code: A Handbook of Agile Software Craftsmanship](https://github.com/zedr/clean-code-python#table-of-contents)
