---
schema_version: "1.0.0"
document_id: "587be78072582161f86c9f06c8243aeda59d3b18205e4a1fc7d08a5669e531f8"
company_key: "yc-redouble-ai"
company: "Redouble AI"
source_id: "yc-redouble-ai-rss-94bf26a72f2a"
canonical_url: "https://deconvoluteai.com/blog/neuralception/pyenvvirtualenv"
published_at: "2023-09-22T00:00:00+00:00"
first_seen_at: "2026-07-27T11:30:56.530208+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:a0470e7af9adfd1af81ef7b0b3fbf69e70cbc9d088de0f45cf3a7b4ac2561b99"
---

# Managing Python environments with pyenv and pyenv-virtualenv

This post was first published on[NeuralCeption](https://www.neuralception.com/pyenvvirtualenv/) .


When working with Python it is a good idea to use separate environments for separate projects.


[Pyenv](https://github.com/pyenv/pyenv) is a Python version management tool which lets you create and switch between different versions of Python on your machine. This is useful for example if you want to run tests of your project on multiple versions of Python.


The` pyenv` plugin[pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv) allows you to create environments. In combination with` pyenv` , you can now create environments with a specific version of Python for a project and install Python packages in it, without interfering with the installed packages you have for a different project. And if you break one environment you can simply delete it and create a new one to start from scratch, without breaking all your projects.


This post shows how to install and use` pyenv` and` pyenv-virtualenv` on a Mac M1 with` bash` .


# Installation


Use homebrew to install` pyenv` by running


```text
brew update
brew   install   pyenv
```


Next, you have to initialize it by adding configurations to your` .bashrc` or` .bash_profile` file by running the following (for` .bash_profile` replace` .bashrc` accordingly)


```text
echo     'export PYENV_ROOT="$HOME/.pyenv"'     >>   ~/.bashrc
echo     'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"'     >>   ~/.bashrc
echo     'eval "$(pyenv init -)"'     >>   ~/.bashrc
```


Now we can install` pyenv-virtualenv` . Simply run


```text
brew   install   pyenv-virtualenv
```


To automatically activate environments, run the following to add a configuration to your` .bashrc` or` .bash_profile` file:


```text
echo     'eval "$(pyenv virtualenv-init -)"'     >>   ~/.bashrc
```


# Creating environments


Before we can create an environment with a specific version of Python we have to install the Python version which the environment should use with` pyenv` . We can see available Python versions with the command


```text
pyenv   install     --list
```


To install for example version 3.11.3, we run


```text
pyenv   install     3.11  .3
```


I've had the experience that some versions don't work on a Mac M1. I've listed the ones I use at the end of the post.


Now, we can use this Python version to create a new environment named` 3.11.3-my-env` with` pyenv-virtualenv` like so:


```text
pyenv virtualenv   3.11  .3   3.11  .3-my-env
```


# Activating environments


To list all versions we have created run


```text
pyenv versions
```


To activate a specific environment, run


```text
pyenv activate   3.11  .3-my-env
```


We can also set a version for a specific folder, so that for example whenever you navigate into the root of a project named` my-project` , the version` 3.11.3-my-env` is used. To do that, run


```text
pyenv   local     3.11  .3-my-env
```


in the path of that project. This creates a file` .python-version` in that folder which holds the version information.


Your system's Python distribution can be activated with


```text
pyenv   local   system
```


# Removing environments


To remove the environment` 3.11.3-my-env` we run


```text
pyenv virtualenv-delete   3.11  .3-my-env
```


# Mac M1 issues


On a M1 Mac some versions of Python did not work. I use the following and can confirm that they do work:


- 3.7.13
- 3.8.13
- 3.9.11
- 3.10.3
- 3.11.3
