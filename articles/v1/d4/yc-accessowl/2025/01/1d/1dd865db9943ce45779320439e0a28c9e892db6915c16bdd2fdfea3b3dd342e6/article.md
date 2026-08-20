---
schema_version: "1.0.0"
document_id: "1dd865db9943ce45779320439e0a28c9e892db6915c16bdd2fdfea3b3dd342e6"
company_key: "yc-accessowl"
company: "AccessOwl"
source_id: "yc-accessowl-news-import-49160e0486d6"
canonical_url: "https://www.accessowl.com/blog/gam-beginners-guide-how-to-manage-google-workspace-at-scale"
published_at: "2025-01-24T00:00:00+00:00"
first_seen_at: "2026-07-23T01:01:37.642407+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:a69e4f5b5dd87273c2d52ed05b052669e9962ddb842bd9a91d688a0f41af1382"
---

# GAM Beginners Guide: How to manage Google Workspace at scale

Google Apps Manager (GAM) is a popular **command-line tool** that provides a **more efficient way for administrators to manage domain and user settings in Google Workspace** . It is open-source and is available for Windows, Linux, and MacOS.


But why would you need a third-party tool to manage Google Workspace, when you already have the Google Admin Console? To quote Jay Lee, the original creator of the tool, “ **GAM exists to save you mouse clicks** .”


## Why use GAM?


Even simple tasks, such as checking a user’s information, require multiple clicks and navigating different sections of the Admin console. GAM, on the other hand, will return the data in seconds — with one simple command.


However, GAM's biggest strength is its ability to run multiple commands in parallel.


For example, say you have 10 admin tasks that you need to complete for 100+ users. That would take a lot of time and mouse clicks using Google’s admin console.


With GAM, you could create a text or .csv file running all the specific commands for each task at once. Even better — you can save the scripts, and simply run them again any time you want to perform specific tasks, saving you hours of repetitive busywork.


Intrigued yet? Follow along as we cover all the basics you’ll need to get started with Google Apps Manager, including installation, some classic use cases, and security considerations.


## Getting started with GAM: Do you need special training?


All you need to start using Google Apps Manager is familiarity with command line tools and a general understanding of how[GAM commands](https://www.accessowl.com/blog/gam-beginners-guide-how-to-manage-google-workspace-at-scale) are structured.


Start by practicing single commands on single workspace objects.Then you can gradually move to using CSV for bulk commands on multiple objects. You don’t need to know every GAM command that exists before you get started. Just become familiar with the ones you need to use in your daily tasks.


The good news is: both versions of GAM have comprehensive resources outlining all the different commands for various administrative tasks.


Speaking of which, there are two popular and trusted versions of GAM — the original GAM by Jay Lee and a forked version, GAMADV-XTD3, by Ross Scroggs.


## GAM vs. GAMADV-XTD3


GAMADV-XTD3 is an extended version of GAM, so it comes with additional features and functionality beyond what’s available in the original GAM.


However, this shouldn’t be a major concern when starting. Regardless of which version you use, it will be a game changer for your workspace-management tasks.


Both Jay and Ross are always actively updating the tools to add new functionality, and they’ve built a thriving community of users ready to help you with any query.


As you advance your skills and engage with the GAM community, you can make a more informed decision on which specific version better suits your unique needs.


## Setting up GAM: How to install Google Apps Manager


While the installation will be different depending on your operating system, it follows the same basic steps. The installation process is also quite similar for both the original GAM and GAMADV-XTD3.


For demonstration purposes, I’ll show you how to set up Jay Lee’s GAM on Windows.


You’ll need super admin access to the Google Admin Console, as you must authorize GAM and grant it API permissions.


You'll also need to make sure API access for your domain is enabled on the Admin Console.


### Step 1: Run GAM installer


For Linux and MacOS, open Terminal and run the command:


```text
bash   <(curl  -s    -S    -L
```


This will download GAM, install it, and start setup.


Windows users need to download the[latest MSI installer](https://github.com/GAM-team/GAM/releases) , run it, and follow the installation wizard to completion. The installation may get flagged by Windows Defender or your antivirus. But that’s normal, so don’t worry about it.


Once installation is complete, you’ll be prompted to enter your workspace super admin email to set up a Google API Project and authorize GAM for admin management.


### Step 2: Creating a project


If GAM doesn’t automatically prompt you to set up a project and installation, use this command to start project creation.


```text


```


You’ll be prompted to enter the Google Workspace admin email that will be used to control GAM.


GAM will then authorize all the necessary APIs and output a URL that you need to open manually, by copying and pasting it to your browser. It will also provide the instructions for creating the OAuth client ID and client secret necessary to complete the process.


Enter the generated details into the GAM command line as prompted. You’ll then receive the next set of instructions to enable workspace domain-wide delegation and complete the process.


Press “Enter” when done, and you’ll have finished creating the GAM Project.


### Step 3: Authorize GAM admin access


You need to authorize GAM to act as your Workspace Administrator to perform management functions — like adding users, modifying group settings, and pulling domain reports.


If you’re not automatically prompted, use this command to start the process.


```text


```


Enter the workspace admin email you used previously to continue.


You'll be presented with a long list of APIs that GAM can use. By default, the most important APIs are selected. Unless you know what you’re doing, just leave this selection as is, and press “C” to continue to authorization.


GAM will either open a web page for you or prompt you with a URL to visit to authorize admin access. Choose your account, and allow GAM to access and modify it.


### Step 4: Authorize user data and settings access


This process enables GAM to act on behalf of your users, to modify user-specific settings and data such as Drive files, Calendars, and Gmail signatures.


Use this command **** if you’re not automatically prompted to authorize user data access as part of the installation process.


```text


```


To complete this step, enter the email of any regular non-admin user in your Google Workspace domain. Ensure this user has the Gmail, Drive, and Calendar services enabled.


Now GAM will attempt to authenticate with each service/scope via the service account and act on behalf of the user you’ve specified. It's expected that the first check will FAIL.


Go to the URL provided, and grant additional access to the scopes provided. If the check fails again, give it more time and recheck.


And you’re done — GAM is installed and ready to go. If you run into additional trouble, the[GAM Wiki](https://github.com/GAM-team/GAM/wiki) is always a great resource for you to reference. (Or use the[GAMADV-XTD3 Wiki](https://github.com/taers232c/GAMADV-XTD3/wiki) , if you’re installing that version.)


## Common GAM Commands for everyday workspace management tasks


I’m not even going to try to cover all GAM commands here, as it would be nearly impossible. Instead, we’ll briefly explore a few commands for everyday workspace management tasks.


You can access the full GAM commands list on the GAM Wiki. There’s also a[GAM cheat sheet](https://gamcheatsheet.com/GAM%20Cheat%20Sheet%20A3.pdf) , which will be invaluable once you’ve understood the basic structure of GAM commands.


Here are examples of 10 areas where you can leverage GAM to make your work easier as a Google Workspace administrator.


### 1. Edit, Create, or Delete Users, Groups, or Organizational Units (OUs)


Some basic commands in this category include:


-


Command to get information — like names, customer ID, last login, license status, mailbox status, etc. — for users on your domain.


```text


```


-


Command to get information on groups, such as group members and their permissions.


```text


```


-


Command to create a new user account.


```text


```


-


Command to delete a user.


```text


```


You can take these commands to the next level by including additional parameters, depending on what you’re trying to achieve.


For instance:


-


Here’s a command to update groups with members from any organizational unit you specify. You can replace “member” with “owner” or “manager” to change roles.


```text


```


You can also choose to update the group with one specific user, users from another group, users listed on a CSV file, or all the users in your domain, by modifying the “ **ou”** parameter.


But you may find it easier to start light, and then expand your commands as you grow your GAM skills. Learn a few commands that you need regularly, and build up from those.


### 2. License management


-


Command to assign a license for the specified SKU (product ID) to a user or number of users. You can modify the “ **user** ” parameter to assign the license to a group, organizational unit (OU), or all users in the domain.


```text


```


-


Command to ensure that all users in the specified organizational unit have the same licenses assigned to them. If a user has a different license, it will be revoked.


```text


```


-


Command to update a group’s license.


```text


```


### 3. Email management


-


Command to access the email inbox for all users in your domain and delete any message from a specific sender, with a specific subject, or certain recipients.


```text


```


-


Command to delegate email access to a specified user. The delegate and the delegator must be in the same domain.


```text


```


-


Command to set a custom signature for all users in the specified organizational unit, based on a file you created. It will also replace the placeholder "{company}" with "AccessOwl"


```text


```


### 4. Drive management


-


Command to generate a spreadsheet containing a list of all externally shared files owned by your users to your admin’s MyDrive.


```text
gam all users print filelist query  "visibility='anyoneWithLink'"
```


-


Command to display all files for all users that contain the text "ProjectX"


```text
gam all users show filelist query  "fullText contains 'ProjectX'"
```


### 5. Calendar management


-


Command to assign the specified “user email” editing access to the calendar owned by the specified “calendar email.” It will then notify the users of the changes.


```text
gam calendar <calendar email> add editor <user email> sendnotifications  true
```


-


Command to remove “user email” rights to the specified calendar.


```text


```


### 6. Device management


-


Command to show all devices in the domain


```text


```


-


Command to add a new device to the Google company-owned inventory using the provided information.


```text


```


-


These command variations can be used, respectively, to delete, wipe all device data, cancel a pending wipe, approve, or block a user profile on a device.


```text


```


### 7. Security and compliance


-


Command to generate a user activity report for the specified parameters and save it to Google Drive in CSV format.


```text


```


Here is an example to generate a Google Sheet of Google Meet total usage across your users.


```text
gam report usage customer parameters meet:total_call_minutes,meet:total_meeting_minutes todrive start_date  2020  -03  -01   skip_days_of_week sat,sun skip_dates  2020  -03  -06
```


### 8. Monitor the third-party applications of your users


Command to upload a CSV report of OAuth token activities for the specified user to Drive (a great way to determine if your users are connected to any unauthorized application).


```text


```


### 9. GAM Google Chatbot to send alerts


GAM is capable of acting as a Chatbot and sending messages to Chat Rooms or direct messages to users. For instance, you can run GAM on a Windows server and have it start a chat if the server hits a high load.


Here's how[to set up a GAM Chatbot](https://github.com/GAM-team/GAM/wiki/Chat-Bot) .


### 10. Automating routine tasks


At the advanced level, you can combine multiple commands to automate various processes, like user onboarding and offboarding.


For instance, you can develop a user onboarding process — create the user, send them their passwords, and add them to relevant groups — and create a user provisioning script that you run every time you onboard a new user. You can also do the same for onboarding.


## GAM security considerations and mitigation


GAM’s powerful administrative capabilities are also what makes the tool a potential security risk. Considering it can access and modify user inboxes as well as Drive, GAM could have devastating consequences in the hands of a malicious user.


Here are measures to safeguard your GAM installations and prevent unauthorized access:


1.


Avoid leaving your GAM installations unmonitored.


2.


Consider running GAM on a dedicated Virtual Machine (VM) or using Google Cloud Shell.


3.


Practice the principle of least privilege, by setting up Google Cloud Projects with minimal permissions. Or…


4.


Create separate installations of GAM to delegate capabilities, ensuring users have access only to the functions they need.


5.


Secure authentication tokens: The authentication tokens GAM generates during setup remain valid until the associated Google Cloud Project is deleted. Ensure that you store and share them securely.


## Additional resources


-


[GAM Public Chat](https://groups.google.com/a/s.jaylee.us/g/gam-public-chat) : A community where you can get help on using GAM and collaborate with other Google Workspace Admins on just about anything.


-


[GAM for Google Workspace](https://groups.google.com/g/google-apps-manager) : A forum for general discussions regarding GAM.


## Conclusion


So there you have it — a comprehensive guide on how to make the most out of Google Workspace with Google Apps Manager. But what if GAM just isn’t for you, because you prefer working with a graphical user interface?


Well, Google Admin Console still has everything you need. You can also combine it with another tool like[AccessOwl](https://www.accessowl.io/) to simplify various tasks such as third-party app monitoring, access management, and reporting. Like the Admin Console, every action is performed on an intuitive interface that doesn’t call for advanced technical skills.


## FAQ


### What is GAM (Google Apps Manager)?


GAM is an open source command line tool for Linux, MacOS, and Windows, which helps to simplify domain and user settings management in Google Workspace. Administrators love it, because it saves them clicks and allows them to quickly and efficiently complete tasks through scripts.


### Is Google Apps Manager safe to use?


Yes. GAM is a tool trusted by tens of thousands of Workspace admins to simplify their roles. Moreover, the tool was created and is maintained by Jay Lee — who’s currently working at Google as the strategic cloud engineer for Workspace. Also, GAM is open-source — meaning you can check it for malicious code.


### Is GAM better than the Google Admin console?


GAM is faster and more efficient than the Admin Console, because it uses command line prompts instead of mouse clicks. However, it’s not a replacement for Admin Console — which is the dedicated Workspace management tool featuring an intuitive UI.
