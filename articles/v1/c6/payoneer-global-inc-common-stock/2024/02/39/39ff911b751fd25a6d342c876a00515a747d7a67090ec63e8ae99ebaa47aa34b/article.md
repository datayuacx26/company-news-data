---
schema_version: "1.0.0"
document_id: "39ff911b751fd25a6d342c876a00515a747d7a67090ec63e8ae99ebaa47aa34b"
company_key: "payoneer-global-inc-common-stock"
company: "Payoneer Global Inc."
source_id: "payoneer-global-inc-common-stock-rss-9fa92a296e72"
canonical_url: "https://engineering.payoneer.com/fiddler-extensions-primer-7f602b63db78"
published_at: "2024-02-19T06:59:06+00:00"
first_seen_at: "2026-07-20T23:18:22.924920+00:00"
fetched_at: "2026-07-28T22:26:15.382818+00:00"
content_hash: "sha256:6b985315288f158b90487fdf75c4d6697cc8881819b872768bdc7133bf0ace80"
---

# Fiddler extensions primer

# Fiddler extensions primer


[Michael Berezin](https://medium.com/@mbearz?source=post_page---byline--7f602b63db78---------------------------------------)


10 min read


·


Feb 19, 2024


--


Press enter or click to view image in full size


In today's world, when everything is moving to the cloud and microservices, most developers find themselves working with systems that send HTTP requests.


As a result, developers need a tool that allows them to interact with HTTP requests that are going through their machines. There are countless scenarios where we may want to:


1. Modify requests by redirecting them to a different URL.
2. Add or remove headers.
3. Encode the request body.
4. Decode or deserialize the response body.
5. Save the requests for future use (for example, to send to a different team member so they can see what we’re doing).


One of the tools that help us do all of this is[Fiddler Classic](https://www.telerik.com/fiddler/fiddler-classic) .


Fiddler is a network proxy. It stands between your applications and your network card and allows you to monitor and modify (and even block) network requests.


Fiddler is very extensible, just by using its built-in script editor ([read my previous article about doing just that](https://medium.com/p/e4e9ccdfd052/edit) ), but often we need to be able to do things that are impossible to do in the script editor. For example, we may need to get the IP address of our machine, save or retrieve data from a DB or an API, or decode a[JSON Web Token (JWT)](https://jwt.io/) .


Luckily for us, Fiddler is very easy to extend using Visual Studio and a basic knowledge of C#.


## Building an extension


Before we get to the technical details, let's talk about the scenario that we need an extension for:


We have a microservices-based system, which is behind an API gateway. If we need to debug a specific service, we can add a header to a request ( *x-service-debug-redirect* ). The value of the header should be a comma-separated list of service names and an IP address. This will tell the API gateway that in the flow of this request, for each service that is named in the header value, to redirect the request to the specified IP address instead of the deployed service.


For example, if we add the header *x-service-debug-redirect: login;127.0.0.0.* then the API gateway will redirect requests for the LoginService to 127.0.0.0 instead of sending the requests to the regular LoginService that’s deployed.


In this tutorial, we’ll create an extension that will allow us to define a list of services to select one or more of them to add to the *x-service-debug-redirect* header for outgoing requests.


Writing a Fiddler extension can be split into 3 parts:


1. Create an extension library and interface with Fiddler.
2. Write your custom code.
3. Inject the extension to Fiddler.


**Create an extension library and interface with Fiddler**


**Note:** you can[check out this repo](https://github.com/payoneer/FiddlerExtensionPrimer) to see the full code for this extension.


First, we need to open Visual Studio and create a new C# class library (.NET framework), as Fiddler was built using .NET framework its extensions should be .NET framework as well.


The first step of connecting your app to Fiddler is adding the Fiddler.exe file as a reference for the project. Right-click the **References** node in the solution explorer window.


Then click on **Add Reference…** This will open the Reference Manager. Click on the **Browse…** button in the bottom right corner, this will open the file explorer and allow you to go to the Fiddler installation path and select the Fiddler.exe file. After you have done that, you should see the Fiddler reference under the references node


Press enter or click to view image in full size


Press enter or click to view image in full size


**Write your custom code**


Now that we have a reference to Fiddler we can start using it in our code.


Go to the *Program.cs* file in your app and give it a better name (in this example we’ll use *FiddlerPrimer.cs* but feel free to select your own name).
Replace the code in *FiddlerPrimer.cs* with the following code:


*IAutoTamper* is an Interface we get from Fiddler.exe, which allows us to interact with several hooks in the request lifetime. We will add code to these hooks later in this tutorial.


Next, add the *Newtonsoft.Json* NuGet to the project (right-click on the project and select **Manage NuGet Packages** . In the NuGet Manager window go to the **Browse** tab and enter Newtonsoft.Json in the search bar. Once it is found, select it and click the **Install** button on the right side.


Press enter or click to view image in full size


Once that’s done, right click the **References** node in the solution explorer window then click on **Add Reference** ** to open the Reference Manager. Go to the Assemblies tab and search for *System.Windows.Forms* . Once it’s found, select it and click on the OK button.


Press enter or click to view image in full size


**Note:** *Newtonsoft.Json* and *System.Windows.Forms* are not strictly needed for a Fiddler extension, but we will use them in this primer. If you’re creating your own extension, you can omit them if they aren’t needed.


As we need to add a list of service names to a header, we need to enter those names somewhere, but we don't want to do it every time. So, we need to have somewhere to save them. The first step in achieving that is adding a file *ServicesSettings.cs* . Add the following code to it:


The class *ServicesSettings* will be used to hold the setting for this extension. It currently only has a list of service names, but we can easily extend it if needed by adding more properties.


Now we need a way to save and retrieve these settings to somewhere permanent, and Fiddler can help us.


Create a file named *ServiceSettingsHandler.cs* and add the following code to it:


The main thing to note here is that we are using *FiddlerApplication.Prefs* **** to store and retrieve our data from the same storage that Fiddler uses to store its own preferences. Both the get and set methods get a preference key, and we’ve created a constant variable *PrefName* **** to define it (you should give the access a well-defined and specific name to avoid colliding with other extensions).


The second thing to note is that we’re using *JsonConvert* from the *Newtonsoft.Json* NuGet that we have installed before, to serialize and deserialize our settings object to a string and back.


Now that we have a way to save our settings, we need to create a UI to input them. As this is a primer about Fiddler extensions and not C# UI development, we are going to create a quite simple UI (you can improve it later as a nice learning exercise).


To add a new UI form, we need to right-click the project in the solution explorer and click on **Add** , then **New Item** *.* In the new items dialog, select the **Forms (Windows Forms)** option, enter the name *ServicesSettingsForm.cs,* and click on the **Add** button.


Press enter or click to view image in full size


This should create an empty Form file and open its editor window. As we want to keep the UI simple, we only want to add a rich text editor.


## Get Michael Berezin’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


Open the **Toolbox** pane (if it isn’t open, click on **View** in the topmost menu and look open the toolbox from there).


Open the **Common Control** note, look for *RichTextBox,* **** and drag it to the form.


The *RichTextBox* will be added in a single-line mode (as this is the default). Right-click it and then click on the **Properties** option. This will open its properties window. In the properties, window look for the *Multiline* value and change it to **True** .


This will allow us to insert several lines into the editor and change its size in the editor.


Select the form in the editor and press F4 again to see the form properties. Move to the events tab, and look for the *FormClosing* event. When you find it double click on the row to have Visual Studio generate a method to handle this event.


This should create a method named *ServicesSettingsForm_FormClosing* (given that ServicesSettingsForm is your form name) and take you to the code editor for the form. You can select ServiceSettingsForm.cs in the Solution Explorer and press F7 to go to its code anytime you need to.


Replace the code in *ServiceSettingsForm.cs* with the following code:


In the constructor, we load the current settings, and if the settings already have some items we add them to the text editor, with each setting on a new line.


In the *Form closing* event, we get the text from the editor and split it, so each line is a separate item, then we save each line item as a separate *ServiceModel* while setting the *IsActive* flag to True for any item that we already had in our list and was set as active.


Now that we have a UI to save our settings, we need a way to have Fiddler open this UI when we need it, and the best way to do this is by adding a new option to the Fiddler main menu and binding some code to it so it will open our UI.


Go to *FiddlerPrimer.cs* and ** add the following code to the top of the *FiddlerPrimer* class. This code will create a new menu item and tell it to open our UI when it is clicked:


Now go to the *OnLoad* method and add the following 2 lines to it:


```text
_serviceMenu = CreateServiceMenu();   FiddlerApplication.UI.mnuMain.MenuItems.Add(_serviceMenu);
```


This will create the menu item and add it to the Fiddler main menu when Fiddler is starting.


Now we can add services to our settings. We need a way to tell Fiddler to add our header to each outgoing request, for services that we have activated.
Go to the *AutoTamperRequestBefore* method and add the code below.


The code gets the settings, and if we have active services to add to the *x-service-debug-redirect* header, we add the service name to a comma-separated list with the PC IP address (we also get the IP straight from the DNS).


**Inject the extension to Fiddler**


Now that we’re done with the code, we only need to make Fiddler load our extension.


We can easily do this by copying all the DLL files from our app bin/debug folder and adding them to the Fiddler scripts folder (the default path is *%USERPROFILE%\\AppData\\Local\\Programs\\Fiddler\\Scripts* ).


While we can do this manually, we don’t want to do this for every change in the code. Luckily, we can have Visual Studio do this for us.
Select the apps project in the solution explorer and press **ALT+ENTER** to open the properties window. Go to the *Build Events* tab, and paste the following to the *Post-build event command line window* :


```text
for /R “$(TargetDir)” %%f in (*.dll) do copy %%f “%USERPROFILE%\AppData\Local\Programs\Fiddler\Scripts”
```


This will tell Visual Studio to copy all the DLL files to Fiddler's script folder.


The *Post-build event* script will be executed every time we build or re-build our project, so it will ensure that our extension is always up to date in Fiddler.


**Note:** in some cases, Visual Studio is not able to copy files while using the **%USERPROFILE%** shortcut (for example if it’s mapped to a network drive). For those cases, you will need to use the full path.


When we now build the project we should see a message in the output window that our files were copied.


In case you want to debug the extension while Fiddler is running, open the project properties window and go to the *Debug* tab. Now enter the path to Fiddler.exe in the *Start external program* line.


Press enter or click to view image in full size


After this, you can start the project, and Visual Studio will start Fiddler and bind your code to the Fiddler process so you can debug your code.


All that is left to do now is open Fiddler and confirm that we have the services option in the menu.


If we click on **Services** , **** we can see the services that we already have marked as active and an **Edit** button.


Clicking on the **Edit** button will open the UI form we created with the services we have defined.


Now that the login service is active, every outgoing request will have the *x-service-debug-redirect* added to it just like we wanted.


Press enter or click to view image in full size


If you want to see what extensions Fiddler is using, open Fiddler and go to **Tools > Options** .


Open the **Extensions** tab.


Press enter or click to view image in full size


Here you can view all the extensions and where they are located.


[Check out this repo](https://github.com/payoneer/FiddlerExtensionPrimer) to see the full code for this extension.


**Conclusion**


While this scenario is not very complex, it still illustrates how time-saving a Fiddler extension can be. Just think of the amount of time you would spend over a year looking up your current IP address, and manually adding headers to outgoing requests.


Fiddler extensions have many other useful applications.[Here’s some Fiddler inspiration](https://www.fiddlerbook.com/Fiddler2/extensions.asp) using some of the interesting extensions.
