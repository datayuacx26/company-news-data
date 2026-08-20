---
schema_version: "1.0.0"
document_id: "2564bfe482d25000da943bdc54e5155f67de8668ab295e0b19dedc8021e956af"
company_key: "yc-microverse"
company: "Microverse"
source_id: "yc-microverse-news-import-bead39b6c183"
canonical_url: "https://www.microverse.org/blog/how-to-structure-a-react-native-app"
published_at: "2023-06-21T00:00:00+00:00"
first_seen_at: "2026-07-22T04:19:00.975368+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:367381d98d7015171670dc1e66593652c28b75645e8e2ea9dcca79c6f8e9f2b2"
---

# How to Structure a React Native App: React for Beginners

## What is a React Native App?


React Native is a famous JavaScript-based mobile application framework that you can use to build natively rendered mobile applications for iOS and Android. Using the same codebase, the framework enables you to develop an application for various platforms creating a React Native app.


## How does a React Native App work?


React Native is created by a combination of JavaScript and JXL, a unique markup language resembling XML (eXtensible Markup Language). The framework has the capacity to interact with both existing native app threads and JavaScript-based threads.


So, how does this interaction operate? React Native makes use of a[bridge](https://hackernoon.com/understanding-react-native-bridge-concept-e9526066ddb8) . The React team created the concept of React Native Bridging to assist mobile app developers in creating their own custom modules when the React team's default components are not sufficient. Despite the fact that JavaScript and Native threads are built in entirely separate languages, bidirectional communication is made possible thanks to the bridge feature.


### How to create a react native app from scratch?


To start, you need a development environment to be able to create a project from scratch. Setting up a suitable development environment will involve a few different steps depending on the type of OS you have. Whether it is Windows, Mac, or Linux, they are largely the same but involve a few different steps.


Installing Node and the required applications for Android and iOS is the typical installation process. Note that running your app on an iOS simulator is impossible if you use a Windows or Linux machine. For macOS, there is iOS.


#### Step 1: Installing Node


Installing[Node](https://www.npmjs.com/package/react-native-node) is the first thing that has to be done. A server-side JavaScript runtime environment is called Node. It is responsible for constructing our JavaScript code. Node will bundle our Javascript using a server-side build script called[Metro](https://facebook.github.io/metro/) , specifically for React Native.


Metro gives you a packed JavaScript file with all of our code, options, an entry file, and other information. One of Metro’s key features is sub-second reload times, which enable the fast refresh feature. Therefore, all we have to do to include the package is run.


**MAC**


{% code-block language="js" %}
$ brew install node
{% code-block-end %}


Homebrew, or "brew," is a package manager designed specifically for MacOS, making it incredibly simple to install programs.


**Windows**


[Powershell](https://learn.microsoft.com/en-us/powershell/) is used for Windows, and Choco, Windows' recommended package manager, is used for installation. Simply launch a PowerShell session with administrator privileges. This will immediately install the Java Development Kit (JDK) and Node for us. Don't touch it; it will take some time! It's not actually frozen.


{% code-block language="js" %}
$ choco install -y nodejs.install openjdk8
{% code-block-end %}


**Linux**


Depending on your[Linux distro,](https://www.techradar.com/best/best-linux-distros) you can simply follow the steps[here](https://nodejs.org/en/download/package-manager/) . Simply choose the proper distribution and follow the instructions.


#### Step 2: \[Android\] Installing JDK & Android Studio


The JDK must first be installed. Giving developers the resources they need to write Java programs is the responsibility of the JDK. The program Android Studio, which will host our emulators, must also be installed.


Let's start by installing JDK. The command below will require us to access our package manager, Homebrew. When prompted, simply key in your password. It is advised to download from[AdoptOpenJDK](https://adoptopenjdk.net/) or your preferred package manager for Linux.


*{% code-block language="js" %} $ brew install --cask adoptopenjdk/openjdk/adoptopenjdk8 {% code-block-end %}*


#### Step 3: Running our React Native Application


Finally, we can build our React Native application using the command line interface (CLI). Create a React application by using the magic command, which is:


{% code-block language="js" %}
$ npx react-native init FirstProject
{% code-block-end %}


Although we can start a new project with a variety of predefined templates, for this example, let's start from scratch.


The command will create a folder for us on the drive where the terminal was opened after it has been executed. So, allow it to finish before moving on to the next action.


We have now reached the final stage of running our project on an Android emulator. But, we need to confirm that the dependencies were set up before launching.


So let's open a new terminal in the directory where our React Native application is located. Then simply execute the command:


{% code-block language="js" %}
$ npm install
$ npm run android
{% code-block-end %}


The Metro Bundler will then appear in a new terminal window. The Metro Bundler is responsible for packaging all of our JavaScript files into a single file. We should be almost done when the emulator launches automatically and installs the React Native application onto the phone.


## Structure the pillars


The best method for developing libraries that make use of platform-specific APIs is to use Native Modules. Using native components is also the best way to develop reusable UI components that provide consumers a natural experience.


[Image source](https://miro.medium.com/max/1400/0*2UvHBt3DhXGKLe9D.png)


Our attention would be on the src folder because the other files and directories above (some abbreviated) are typically a part of a normal react-native initial installation:


To create complex user interfaces using the fundamental building elements, components are combined. The simplest building part is known as a "View," a component on the screen that can show text, and graphics, or react to user input.


For each of its components, React Native generates equivalent native views at runtime. As a result, apps created with Swift or Kotlin have a similar appearance and feel to other native apps.


#### **api/**


The folder contains the logic behind external API communications. Within it are separate feature files. This means each feature file contains the api communication logic needed to implement a certain feature.


#### **assets/**


As the name suggests, this is where the application's static files—such as images—are stored.


#### **components/**


This category contains shared parts that are used by many features. The layout component, which is used to wrap the application's components and decide its overall layout, is an example of one of them (as demonstrated below).


*Image source: Personal library*


#### ‍ **features/**


The feature folder, which makes up a significant portion of this architecture, contains a module for each feature of the program.


#### **reducers/**


This reducer operates on an application level. Its purpose is to use the **combineReducers** function of Redux to combine the multiple feature-level reducers.


This is what reducers/index.js looks like:


*Image source: Personal library*


#### **styles/**


We store our application-level styles in this module. They are then referred to in different components by utilizing the multiple-style syntax of React Native, as demonstrated in the example below:


{% code-block language="js" %}
<View styles={\[ globalStyles.wrapper, styles.textWrap \]}> ...
</View> {% code-block-end %}


#### **index.js**


This is the entry file for the application; it wraps the application in the layout we previously covered and links the application to the store using the higher level provider component of the Redux framework.


## Enable the use of alias


Since we can't use Webpack with React Native, we must write an alias in Babel and set up aliasing using a Babel plugin. First, we want to set up babel-plugin-module-resolver with yarn or npm.


{% code-block language="js" %}
$ npm install babel-plugin-module-resolver
{% code-block-end %}


After installation, locate the.babelrc file in the project's directory and open it. Create the.babelrc file in the root if there isn't one already. Add the following code to the plugins key:


{% code-block language="js" %}
{
“presets”: \[“react-native\];
“plugins”:\[
\[
“module-resolver’’,
{
“root”:\[
“./src”
\],
“Alias”: {
“src”: “./src”,
“Assets”: “./src/assets”
}
}
}
\]
\]
} {% code-block-end %}


Instead of using complicated relative paths like "../../../src/pages/homepage," we can now use "src/pages/homepage."


Follow the commands below to now remove the cache and restart the node server.


{% code-block language="js" %}
$ npm start --reset-cache
$ npm run android {% code-block-end %}


## Enable editors to alias autocompletion


The packager for React Native offers a convenient feature that lets us add a package.json file. Any folder with a name field in a JSON file will have that name as its alias.


You can create a jsconfig.json file at the workspace's root:


{% code-block language="js" %}
{
"compilerOptions": {
"target": "ES6",
"module": "commonjs",
"baseUrl": "."
},
"exclude": \[
"node_modules",
\]
} {% code-block-end %}


## Atomic Components


Consider a developer who has a list of buttons for the program, all of which have a white title and a black background. They need to return to the program to add a new button with progress, because they haven't worked on it in a while.


They may examine the current buttons and alter one to produce the desired outcome, or they could design a brand-new progress button that reduces redundant code, the absence of a style guide, and component inconsistency between view and component.


The Atomic Design file structure is shown below:


[Image source](https://blog.consisty.com/static/0c1d38e917ecc0e6476da68c3b047a5b/d6a46/file_structure_02.png)


Atomic structures divide user interfaces into tiny, straightforward components. They also increase the complexity and consistency of the view's components and code can be reused. Further, atomic structures enhance your application's code readability, scalability, and adaptability and cut down on development time.


## React Scenes


React scenes are a simple technique used to design and test your React components inside of your app.


#### **How to instal** l


React Scenes is more straightforward, user-friendly, versatile, and plug-and-play; it doesn't require a separate build process. Simply run the command below


{% code-block language="js" %}
npm install react-scenes --save
{% code-block-end %}


#### **Usage**


You can point any route to any library, just as you do normal pages because Libraries are a React component that leverages[Scenes](https://github.com/doubco/react-scenes) .


Image soure: Personal library


## Reusable Services


Reusable components in React are UI elements that can create several UI instances across an application.


For instance, a Button component can display various texts on various sites. Generally speaking, we can increase a component's reusability by making it more generic instead of more particular.


## Shared Styles


If you're new to React Native, you might wonder how to share styles effectively throughout your entire application.


In general, you should try to keep as many of your stylistic rules as component-specific as you can. By doing this, the parts continue to be modular and may be relocated or transported easily without utilizing any outside resources.


Although this isn't a firm rule (rather, it's more of an opinion), it's frequently a good idea to think in this way. Maintain as many self-contained and modular components as you can.


There will be instances, though, where your React Native project will need to use global styling. Let’s discuss a few various methods for sharing styles.


#### **Sharing individual values**


This use-case assumes that you have particular values that you want to use in a variety of ways throughout the app.


This means that while these values may be objects that can be applied directly to React Native elements via the style prop, they are not always necessary.


{% code-block language="js" %}
const skyBlue = "#00c0ff"
const darkMango = "#ff5500"
export { skyBlue, darkMango } {% code-block-end %}


#### **Sharing individual styles directly**


{% code-block language="js" %}
Similarly;
const colourClasses = {
blueBackground: {
backgroundColor: "#12aedc"
},
pinkBackground: {
backgroundColor: "#ff67c7"
},
orangeBackground: {
backgroundColor: "#ee803c"
}
}
export { colourClasses } {% code-block-end %}


Consequently, we define something we can use directly within our StyleSheet declarations rather than just describing colors (hex code values).


## Conclusion


React Native is a fantastic tool for building apps that function well on any platform or operating system. Both businesses and developers adore this framework. It is cost-effective and helps you to save a significant amount of work hours, which speeds up the development process.


You can reach a broader audience at once thanks to this framework because one app can be used by both Android and iOS users, the launch can be coordinated, and businesses with a smaller development budget don't have to focus only on one platform.


Structures in projects are more important than you might realize. As your React Native software requires structures, give this good thought so you do not have to rewrite them. Based on your project's size, complexity, and nature, feel free to combine any of the structures we've covered.


Interested in learning more about React or becoming a web developer?[Learn about Microverse!](https://www.microverse.org/go)
