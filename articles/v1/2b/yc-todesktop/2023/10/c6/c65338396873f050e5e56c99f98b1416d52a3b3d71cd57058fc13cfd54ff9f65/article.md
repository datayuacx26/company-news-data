---
schema_version: "1.0.0"
document_id: "c65338396873f050e5e56c99f98b1416d52a3b3d71cd57058fc13cfd54ff9f65"
company_key: "yc-todesktop"
company: "ToDesktop"
source_id: "yc-todesktop-news-import-b82ee277d06d"
canonical_url: "https://www.todesktop.com/blog/posts/how-to-decompile-a-production-electron-app-on-mac"
published_at: "2023-10-27T15:55:00+00:00"
first_seen_at: "2026-07-24T04:14:47.478048+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:418a11dc907a31c7414777bd4e576edc8298de0fbd5a557e62da03e253b6504a"
---

# Decompile an Electron App: A Step-by-Step Guide

In this article, we will guide you through the process of decompiling a production Electron app on Mac or Windows. I've found this useful for debugging issues that only seem to appear in the production app but not in development. It's also a neat trick if you want to look into the source of any Electron Application.


## Prerequisites


Before we dive into the decompilation process, ensure that you have Node.js and npm installed on your Mac. You can download and install them from the official[Node.js website](https://nodejs.org/) .


## Step 1: Install the` asar` Package


The Electron application’s source code is usually packaged in an ASAR archive to improve performance and protect the source code. To extract the contents of this archive, we will use a tool called` asar` .


Install the asar package globally on your system using npm:


```text
npm install -g asar


```


This command installs the asar utility globally, making it accessible from any terminal window.


## Step 2: Locate the Electron App’s Resources Directory


Navigate to the Electron application's resources directory. This is where the ASAR archive is located.


For Mac users, the path to the resources directory is in the following format:


```text
cd /Applications/YourAppName.app/Contents/Resources


```


For Windows users, that path should look like this


```text
cd %AppData%\Programs\YourAppName\resources


```


Replace "YourAppName" with the name of the Electron application you are working with.


## Step 3: Extract the ASAR Archive


Once inside the resources directory, you can extract the contents of the ASAR archive. The ASAR file is named app.asar. To extract it, run:


```text
asar extract app.asar app


```


This command creates a directory named app, containing all the files from the app.asar archive.


## Step 4: Enable Live Editing of the Application


Now the fun part, we can make changes to the newly created "app" directory and those changes will be reflected in the app. In order to make changes to the application and see the effects in real-time, you need to ensure that Electron loads the extracted files instead of the ASAR archive. To do this, rename (or delete) the original ASAR file:


```text
mv app.asar original-app.asar


```


With this change, when you launch the Electron application, it will load the files from the "app" directory instead of the ASAR archive. You can now edit the contents of this directory and relaunch the application to observe the changes.


## Step 5: View Main Process Logs while the app is running


Finally, it can be handy for debugging to see log messages from the main process. To do this then you can launch the binary directly from the terminal. Here's how you do it on Mac:


```text
/Applications/MyApp.app/Contents/MacOS/MyApp


```


And on Windows, just` cd` into the correct directory and you can execute the app directly:


```text
cd  C:\Users\YourUserName\AppData\Local\Programs\YourAppName
"YourAppName.exe"


```


## That's it


This is a super useful debugging technique for your own app. If you are using it with someone elses app then please respect copyright and licensing agreements. Happy coding!
