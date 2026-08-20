---
schema_version: "1.0.0"
document_id: "6e0d8d3e841636930f262bf118dc1b8219b5fc150470222c521406fabd77aa51"
company_key: "yc-mason"
company: "Mason"
source_id: "yc-mason-news-import-529b235964fd"
canonical_url: "https://www.bymason.com/blog/march-updates-remote-adb-new-cli-mason-os-2-9-0"
published_at: null
first_seen_at: "2026-07-22T03:32:07.336551+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:260992abb2cea5eb0ff5e2d0c4e8feecbd785e146bb4ba61072697c29dc9e506"
---

# March Updates: Remote ADB + New CLI + Mason OS 2.9.0

With February now in the rear view mirror, we’ve got some exciting new product announcements to share as we roll into Spring!


These MAJOR updates include a new look for the[Mason Controller](https://platform.bymason.com/controller) product, a revamped[CLI](https://github.com/MasonAmerica/mason-cli/releases) workflow, a new[Mason OS version](https://docs.bymason.com/releases/) , and the star of the show –[our new X-Ray feature.](https://bymason.com/2020/03/05/remote-adb-with-mason-x-ray/)


You can start testing out these new features TODAY – check out below to learn more.


## Remote ADB with X-Ray


One of the biggest challenges our customers face is supporting devices that are deployed in the field. This can be a huge resource drain that takes away from building and shipping awesome new features. In the Android world, one of the best ways to troubleshoot bugs is with Android Debug Bridge (ADB). Unfortunately, this doesn’t work when devices are already deployed because you need to physically plug into the device… until now.


Our new X-Ray feature lets you *remotely* execute ADB commands, such as pulling logs, copying or pushing files, installing/uninstalling apps, initiating a shell, etc.


Here’s what makes X-Ray awesome:


- **You can use it remotely,** meaning you don’t need to physically access the device *or* require a wired connection.
- **You can use it in real time,** allowing you to see logs or interact with the device as issues are happening.
- You can use it **programmatically,** so you can run commands across large groups of devices with ease.
- **It’s securely enabled by default,** meaning you don’t have to turn on developer options.


[You can read more about X-Ray in our blog post](https://bymason.com/2020/03/05/remote-adb-with-mason-x-ray/) – and make sure to keep an eye out for future releases that will let you remotely view and control any device in your fleet.


## CLI Version 1.5


This new version has some significant improvements and changes to the overall workflow for how you build projects with Mason.


You no longer need to manually register each artifact (e.g. apps or media files), and you no longer have to register your project and then build it. Now, when you initiate a mason project, the .masonrc file tells the CLI where all the relevant artifacts are located. This enables the \`mason register project\` command to automatically register all the latest artifacts, and start a build in a single step.


Here’s a look at how the workflow changes for creating a new Mason Project:


**Old Workflow**


1. **Create Project:** Create a project in the Mason Controller.
2. **Manually Register Each Artifact:** Individually register any applications or media files you want associated with your project.
3. **Create YML File:** Create a new config file with any OS configurations, apps, and media artifacts included in the config, and version up your config.
4. **Register Project:** Register that YML file.
5. **Build Project:** Build your project, including your custom Android OS defined by your YML file.
6. **Deploy Project:** Deploy your project to a group or groups of devices


**New Workflow**


1. **Create Project:** Create a project in the Mason Controller.
2. **Initialize Project:** Initializing the project using the Mason CLI automatically creates your mason.yml file, and asks for the paths to any app or media artifacts you will want to register as part of your project.
3. **Configure OS:** Edit the YML file that has been created for you with any OS configs, apps, and media artifacts
4. **Register Project:** Register your project (this will automatically register any apps, media files, and configuration files), AND will automatically build your project.
5. **Deploy Project:** Deploy your project to a group or groups of devices


**Some other cool stuff!**


- The new latest keyword lets you replace explicit versions with “latest”. This works in your mason.yml config with the project or app version, and in the CLI when deploying an app or project.
- Command names can be shortened as long as they don’t conflict with other commands. For example, mason register media bootanimation test-1 latest $file can be shortened to mason r m b test-1 latest $file.


## Mason OS 2.9 is here!


A new version of Mason OS is available for you to test and roll out to production. In addition to support for the X-Ray feature, we’ve added some new APIs to the Mason Framework so your app can control the UX, and included some new OS configuration options.


- Mason Framework


- API for dynamically toggling navigation bar
- API for putting device to sleep
- API for global actions menu
- API for enable/disable NFC


- New Configs


- Add config in Android to override power button (setting the value to 100 let’s your app intercept the event and define what happens on a long/short press)


- config_longPressOnPowerBehavior: 100
- config_shortPressOnPowerBehavior: 100


- Add config to enable or disable X-Ray


## A Slick New UI


Last but not least, if you’ve logged in recently, you’ve probably noticed that things look a little different. We’ve completely revamped the UI and created some small UX changes as well to create a new and improved Mason experience.


## Get Started Today


[Sign up for a Mason account](https://platform.bymason.com/controller/signup) today if you’re interested in trying out any of these new features yourself!
