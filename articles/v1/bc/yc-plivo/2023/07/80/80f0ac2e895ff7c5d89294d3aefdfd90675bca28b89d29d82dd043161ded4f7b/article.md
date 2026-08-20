---
schema_version: "1.0.0"
document_id: "80f0ac2e895ff7c5d89294d3aefdfd90675bca28b89d29d82dd043161ded4f7b"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/banking-ivr-menu-best-practices/"
published_at: "2023-07-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:58e8a4f15b05419f724d84f4c1972f4fab6357a391ca374a06c04fbbb12e53e1"
---

# How to Write a Bank IVR Menu

If you’ve ever made a call and heard “For English, press or say 1,” you’ve encountered an interactive voice response system. IVR allows callers to access information by speaking a word (thus the “voice” part of the name) or pressing a key on a phone’s Touch-Tone keypad. IVR can serve up prerecorded messages, route calls to specific departments or individuals, or pull data such as bank balances from systems — all without the need to speak to an agent. IVR systems benefit consumers by getting them the information they want quickly, without having to wait on hold.


A good IVR system is particularly useful for financial institutions, as many customer inquiries can be answered without taking up a representative’s time. Typical information for a bank’s IVR menu might include:


- Choice of languages in which to hear the menu
- Directory of departments, including technical support
- Account information, including balance checks
- Automated bill payment
- Appointment booking
- Callback option, if all representatives are busy
- Informational recording on topics such as
- Services offered Business hours Email addresses Website URL Social media handles Directory of branch locations


‍


## IVR menu best practices


Whether you’re a bank, mortgage lender, stockbroker, or other financial services professional, following these best practices for IVR systems can help you create a better customer experience.


- Menu options — Don’t overwhelm customers with too many options at each level. Fewer choices make things easier for callers, as they have to remember the ones they’re hearing. Use a maximum of five items at each prompt.
- Special keys — If any keys are reserved for special functions, tell callers about them up-front. For example, “At any time you can press or say 9 to return to the previous menu, or press 0 or say ‘operator’ to speak to an operator.” And that’s another best practice — at every level, give customers an option to speak to a customer service representative.
- Menu order — Put the most popular options first in the IVR system, so callers have to spend less time listening for what they want.
- Option order — Present a task followed by its associated action rather than the other way around, so callers can recognize what they want to do and don’t have to remember “what number was I supposed to press?” For example, “To hear your account balance, press or say or say 1.”
- Accepted input — Allow callers to respond not only via keypress but also via voice. Some customers prefer one way, some the other; give people what they want.
- Pauses — Include a pause of a second or so before giving the choices on a submenu. That allows time for people who have the phone up to their ear to shift it back there after pressing a keypad key.
- Confirm menu level — Start submenus with a short message that confirms the caller has reached the place they intended. For example, “OK, balances. Press or say 1 for your checking account balance; press or say 2 for your savings account.”
- Allow barge-in — Allow callers to select an option at any time — don’t force them to listen to an entire menu before they can make a selection.
- Repeat as necessary — By default, repeat the menu if the caller doesn’t make a selection within a few seconds.


‍


We offer more[IVR best practice](https://www.plivo.com/blog/ivr-best-practices/) suggestions in a blog post, including our favorite: Don’t start off by telling callers, “Listen carefully as our menu options have changed.” No one cares.


## A sample bank IVR


Every organization is different, so every organization’s IVR menu will be different too. We put this simple two-level menu together to give you some ideas. Undoubtedly your menu will require more levels. Also, there’s no single right way to map out a menu. In this example, we start out asking for a kind of account, but you might prefer to ask for a kind of task first and prompt for the account afterward.


Plan the entire menu tree before you start coding and before you start recording prompts.


Here’s our sample.


Thanks for calling Our Bank. How can we help you? You can press 0 or say “operator” at any time to reach a representative during business hours.


For personal banking services, press or say 1.
For business banking services, press or say 2.
For account inquiries, press or say 3.
For credit card services, press or say 4.
To apply for a new credit card, press or say 5.
For information on your card’s interest rate and other terms and conditions, press or say 6.


1: OK, personal banking.
For account balances and transaction history, press or say 1.
To transfer funds between accounts, press or say 2.
To make a bill payment, press or say 3.
To open a new account, press or say 4.
To report a lost or stolen card, press or say 5.


2: OK, business banking.
For account balances and transaction history, press or say 1.
To transfer funds between accounts, press or say 2.
To make a bill payment, press or say 3.
To open a new account, press or say 4.
For commercial loans and financing, press or say 5.


3: OK, account inquiries.
For account balances and transaction history, press or say 1.
To report a lost or stolen card, press or say 2.
To dispute a transaction, press or say 3.


4: OK, credit cards.
To check your outstanding balance, press or say 1.
To make a payment, press or say 2.
To report a lost or stolen card, press or say 3.
To dispute a transaction, press or say 4.


## How to create an IVR system with Plivo


[Plivo](https://www.plivo.com/) makes things easy for businesses that want to implement IVR. We’ve written[IVR use case guides](https://www.plivo.com/docs/voice/use-cases/ivr/python#xml-how-it-works) that show exactly how to create an IVR program in any of seven common web development languages, including annotated code samples.


If you’re not already a Plivo customer,[sign up for free today](https://console.plivo.com/accounts/register/) and get started now.
