---
schema_version: "1.0.0"
document_id: "cd2da72384c88f31cde5d1a0886258eeec8ffa701ff00f7dafa67a0fccaebe83"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/creating-the-sails-dot-env-hook/"
published_at: "2022-03-22T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:7c293ac4dbd33a42510c51f0e8180677affe5d952abaddfcda56d6141aa98f2a"
---

# Creating the Sails dotenv hook

## Introduction


I created the[sails-js-hook-dotenv](https://www.npmjs.com/package/sails-js-hook-dotenv) package to solve a problem I was facing in my development with[Sails.js](https://sailsjs.com/) . That problem was the ability to load environment variables (whether through a` .env` file or system stored variables), which is a staple feature of building[12-factor apps](https://12factor.net/config) . Understandably, Sails.js does not have this feature yet since it is still a maturing framework, but it does mean that it is the responsibility of the community to add support for this feature if they wish the framework to succeed.


In this article, I will discuss what was my process for creating the package, what were some of my learnings from the process, what are some limitations of the package, and what I would like to improve and add to the package in the future.


## Creating the package


Before even beginning to develop the package I had to go through the experimentation phase of trying to load environment variables in my Sails.js application. I was having difficulties, and I could not find anything in the official documentation for how to implement such a feature. When I searched online for other developers’ recommendations for what I could do I was met with two common responses:


1. You cannot load environment variables so your best approach is to create configuration files that you could ignore in Git.
2. You can use[sails-hook-dotenv](https://github.com/Taiters/sails-hook-dotenv)


For the first recommendation, while it is technically a workable solution it does not adhere to the 12-factor app principles, and it is more difficult to add in configuration JavaScript files per environment than it is to add` .env` files. I cannot also override the variables set in the configuration JavaScript files with any variables set in the system, which is common practice with technologies such as Docker.


For the second recommendation, the key issues are that the package was last updated seven years ago, and it uses[dotenv](https://www.npmjs.com/package/dotenv)` 0.4.0` whereas the most recently published version is` 16.0.0` (there have been a lot of major version upgrades since then). The nice thing about the package though is that ” technically” it still works.


I decided to reach out to the[original developer](https://github.com/Taiters) of the package to see if they would be open to transferring to me the[package name in npm](https://www.npmjs.com/package/sails-hook-dotenv) and the original[GitHub repository](https://github.com/Taiters/sails-hook-dotenv) . Unfortunately, I was not able to get a response from them, but if anyone does know them I would greatly appreciate it if you could put us in touch. I decided to fork their original repository and start my package with up-to-date support.


My first steps were to update all the dependencies. Taiters’s test suite was sufficient for me to know if there would be any breaking changes. I then changed some formatting to what I preferred and changed some syntax to ES6. At this point, I felt that I had something ready to release as a new package.


From here I knew that there were some further improvements that I wanted to make and I was able to accomplish them all. Things like:


- Standardize the tests with the way officially supported Sails.js packages are tested
- Add a simple CI pipeline
- Make some dotenv package features available via the Sails.js configuration files
- Add more tests


## Learning opportunities


This was my first ever npm package, and I found it to be a tremendous learning experience. I was able to learn about the simple publishing process and also how you can publish to other registries, such as the GitHub registries if you wish to host private packages for your projects. I was also introduced to the Sails.js community, leading to this article and getting to speak at the[2022 Sailsconf](https://sailsconf.com/) . I am very grateful for both of these opportunities.


## Limitations of the package


Unfortunately, there are some limitations to the package. This package allows you to easily load environment variables in your Sails.js application, but it does not let you load environment variables in your configuration files. To do that you would need to import and configure dotenv in *each* configuration file that you wish to load environment variables. This is, unfortunately, a limitation of the Sails.js framework. The` userconfig` hook, which is responsible for loading all of your configuration files, loads before all other hooks. This means that it loads the configuration files before the` sails-js-hook-dotenv` ever gets a chance to load.


A proposed change for this could be to initialize the dotenv configuration as a part of the` userconfig` hook, thus allowing you to load environment variables in the configuration files **and** the application. A “two birds with one stone” sort of scenario. This would, of course, negate the need for my package altogether, but I knew that when making it. My goal is to push for this change since it would move Sails.js one step closer to helping developers create great 12-factor JavaScript applications. I have reached out to Mike about this change as per the in-code documentation recommended but have not received any response as of this writing.


## Plans for the package


My goals for the package are:


- Maintenance of dependencies
- Review changes from other developers
- Automate the npm publishing process in GitHub actions


## Conclusion


If you have any questions, feel free to reach out to me on[Twitter](https://twitter.com/Actually_Connor)
