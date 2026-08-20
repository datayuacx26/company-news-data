---
schema_version: "1.0.0"
document_id: "2217b15b7bb70872d205865fe66b8e01ea9f90cef5f1929033bb15057d971137"
company_key: "yc-bloom-institute-of-technology"
company: "Bloom Institute of Technology"
source_id: "yc-bloom-institute-of-technology-news-import-0e3bf790d8e6"
canonical_url: "https://www.bloomtech.com/article/how-to-write-an-effective-error-message"
published_at: "2024-02-28T00:00:00+00:00"
first_seen_at: "2026-07-23T03:50:24.397534+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:4ca6455f6d12b06eb3852c03da9f2424aecb3aa6ecc1c8b0f1af0a8429e8ec6f"
---

# How to Write an Effective Error Message

Errors are an unavoidable part of software. Unfortunately, they are given little thought by many developers. Most people tend to focus on the "happy path" and spend little mental energy on what to do when something goes wrong. However, writing effective error messages can save yourself, your team, and your end users from enduring more frustration than is necessary when something inevitably goes wrong.


Because the vast majority of programming is spent communicating with the computer, it is easy to overlook that an error message is your chance to communicate with another person. It is a chance to use plain english to help someone else understand what went wrong and what they can do about it. This gives us a ton of flexibility that we can use to our advantage by following a few simple principles.


## Know Your Audience


When you are writing an error message, it is important to remember who will see it. Are you raising an exception because you tried to update a record in the database and it failed, or are you raising an exception because a user entered an incorrect username or password? Those two scenarios are very different, and the error messages should reflect that.


In the first case, the error message should be targeted towards developers. It should include any relevant information you think would be useful to debug the issue. In the second scenario, the error message should convey to the end user that they should retry entering their username and password. There's no need to include any details about hashing algorithms or the like, because it's irrelevant to the end user's goal of signing into the system.


## Explain Why Something Went Wrong


Imagine you are implementing a new feature that allows a user to enter their birthday. Unfortunately, you have a bug in your code - instead of sending the birthday the user entered, you're accidentally sending today's date. When you send this value to your API, you get an error back: "Invalid Birthday."


Your first response to seeing this error is probably "what the heck?", because of course you don't realize you're sending today's date (which the server correctly identifies as an invalid birthday, since the user is definitely not less than 24 hours old, no matter how fast kids grow up these days.)


Eventually you realize your mistake and correct your code, but how much time it took you to identify the mistake depends on many factors: how experienced are you, how focused are you, how familiar are you with working with dates in this programming language, and so on. You may realize it after 2 minutes, but it's also possible you go down a completely wrong path and don't realize it for an hour.


Instead, imagine if the error message was "The birthday 2018-08-14 is invalid." Today's date being in the error message would have likely caught your eye and pointed you in the correct direction toward a fix.


## Explain What Right Looks Like


Now that you have the user's birthday saving correctly, you move on to implementing functionality to allow a user to choose which timezone they're in. After sending a test value to your API, you get the error message "The value PDT is invalid." Kudos to the engineer who included the value received in the error message!


But now we have another problem - it's not obvious what a correct value looks like. Should we be sending in the UTC offset, the full name of the timezone, or something else altogether? There might be documentation somewhere, but not always. We could dig through the code to see what's valid, but even then the answer might not be obvious. Consider the following:


‍


const userTimezone = req.body.timezone;
const validTimezones = await timezoneService.getAll();


if (!validTimezones.includes(userTimezone)) {
throw new Error(\`The timezone ${userTimezone} is invalid.\`);
}


‍


In this situation, we can't just look at the code to determine what a valid timezone looks like. We have to go to the documentation or the code of the timezoneService and hope there are answers there. Consider if the code was as follows instead:


‍


const userTimezone = req.body.timezone;
const validTimezones = await timezoneService.getAll();


if (!validTimezones.includes(userTimezone)) {
throw new Error(\`
The timezone ${userTimezone} is invalid.
Valid choices: ${validTimezones.join(', ')}
\`);
}


‍


This would have cleared things up for us immediately. We would have seen that PDT was invalid, but PST is not. When possible, you should always try to nudge someone in the right direction with your error messages.


## Do Not Hard Code Dynamic Data


Another approach to the timezone error message above would have been to include the url to the documentation in the error message. This is helpful, but it's not as helpful for a major reason - documentation can become stale and incorrect over time.


This is a contrived example, but let's imagine that two timezones somewhere in the world merged into a new, single timezone. If we have a full list of timezones in our API documentation, there's a good chance it won't get updated until this change is brought to our attention. However, our error message above would include this new timezone (and exclude the two decommissioned timezones) as soon as that change was made in timezoneService. In other words, our error message can't get out of sync with our code, because it's generated from our code.


Consider a simpler scenario - we are tasked with increasing the minimum character length of our users' passwords from 6 to 8. Chances are we'll be working on code that looks something like this:


‍


const userPassword = req.body.password;


if (userPassword.length < 6) {
throw new Error('Password must be at least 6 characters in length.');
}


‍


When we update the length check, we'll probably also notice we need to update the error message as well. But it's possible the code we're working on isn't as straight-forward as this. Perhaps there are more lines in between the length check and generating the error message, and so we don't benefit from the visual proximity that we do in this scenario.


Perhaps our mistake will get caught in a code review, or by QA. But some percent of the time, however small, it will slip through and make it to production. I know this because I have personally updated validation code without updating the corresponding error message, and it made it into production. Given enough chances, it will eventually happen. Instead, what if the code looked like this:


‍


const MIN_PW_LENGTH = 6;
const userPassword = req.body.password;


if (userPassword.length < MIN_PW_LENGTH) {
throw new Error(\`Password must be at least ${MIN_PW_LENGTH} characters in length.\`
}


‍


We now have an error message that can't become inconsistent with the code, because the dynamic information within it is generated by our code. There is nothing more frustrating than reading documentation of any sort that is not correct. By constructing our error messages this way, we can guarantee that they will always be accurate.


## Conclusion


Each of these principles won't apply to every error message, and some of them should be completely ignored in certain scenario (don't include someone's password in the error message, for example). By stopping and considering who might see an error that we are throwing and how we might help them along, we will save them from needless frustration. And as an added bonus, often that times person is our future self!
