---
schema_version: "1.0.0"
document_id: "b88c8d5d73ab9dcf135ef2e113b11068d3e9c09fa770210fe5d15057891dc476"
company_key: "yc-jet-admin"
company: "Jet Admin"
source_id: "yc-jet-admin-rss-b7a9205c1031"
canonical_url: "https://www.jetadmin.io/blog/create-multi-step-forms-with-jet-admin/"
published_at: "2023-11-21T14:26:12+00:00"
first_seen_at: "2026-07-20T23:24:03.180334+00:00"
fetched_at: "2026-07-28T21:01:37.179147+00:00"
content_hash: "sha256:21019679e2bc746cf55d4994902db63d86fbb5db582d87d15da17f1397fc0276"
---

# Create Multi-Step Forms with Jet Admin

In this guide we are going to create a multi-step forms with Jet Admin using temporary variables and conditional visibility.


*Feel like watching instead? Click here for a quick video tutorial!*


Temporary variables and[conditional visibility](https://docs.jetadmin.io/getting-started/part-2-intermediate/conditional-visibility) can be extremely useful as it allows for a dynamic and user-friendly data input process.


[Temporary variables](https://docs.jetadmin.io/user-guide/variables) enable the storage of intermediate data between steps, while conditional visibility ensures a streamlined user experience by displaying only relevant form fields based on user inputs, simplifying complex data entry tasks and improving overall form navigation.


###
Create Multi-Step Forms from Scratch


**Step 1** : Start by creating a new page, selecting the blank layout.


**Step 2** : Create Modals and select a style.


**Step 3** : Create a button that will open the Modal window.


**Step 4** : Create containers to host the steps. Add as many containers as necessary for your use case.


**Step 5** : Start by creating the forms inside the container. Input forms, connect your resource and customize them.


**Step 6** : You are free to preselect the status of any step in the multi-step form you are creating, as well as to hide fields using conditional visibility by setting it up to 0.


**Step 7** : Add buttons that allow switching between the steps in the form.


**Step 8** : Fill your cards as necessary using the components and selecting collections (in our case, we will use a pre-built Gallery).


**Step 9** : Add a search bar and the filters you may need.


**Step 10** : Drag and drop the progress bar. This will help users better navigate through the forms, understanding their progress.


**Step 11** : Create a temporary variable to inform Jet Admin about the current step in the form.


Connect the slider to reflect the current step using the created variable.


Ensure that each button changes the variable corresponding to the current step.


**Step 12** : Configure visibility settings based on the current step variable for all three containers using[Formulas](https://docs.jetadmin.io/user-guide/computed-columns/formulas) .


Don't forget to check the "Load content when hidden" button so the card will show up according to your settings and the user's progress.


### How to Create Powerful Forms?


### Identify Form Sections


Start by identifying the distinct sections or categories of information you want to collect. Break down your form into logical steps to provide a smooth progression for users.


### Add Steps to Your Form


Jet Admin allows you to easily add steps to your form. Simply drag and drop the form elements you need for each section, organizing them into separate steps.


### Configuring Form Elements


Take advantage of Jet Admin's drag-and-drop functionality to add various form elements such as text fields, buttons, dropdowns, filters, and more to each step.


### Set Validation Rules


Ensure data accuracy by setting validation rules for each form field. Jet Admin provides customizable options to define the type and format of data accepted.


### Enhance User Experience


With Jet Admin, you can implement conditional logic to show or hide form elements based on user input.


Tailor the form dynamically by adjusting the visibility of fields based on previous responses. This not only streamlines the user experience but also allows for more efficient data collection.
