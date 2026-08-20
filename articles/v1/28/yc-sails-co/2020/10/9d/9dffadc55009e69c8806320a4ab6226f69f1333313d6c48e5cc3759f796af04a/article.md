---
schema_version: "1.0.0"
document_id: "9dffadc55009e69c8806320a4ab6226f69f1333313d6c48e5cc3759f796af04a"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/understanding-sails-policies-and-best-practices/"
published_at: "2020-10-29T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:3ee2b6008c045f08baa9a73013dbe154ab5aa53bbd5b9696ba75250c783efd92"
---

# Understanding Sails policies and best practices

Often times in an application, you will want to provide access-control - deciding who gets to see and access what. A naive implementation of access control is to try to hide certain interactive elements like a button. The problem in doing so is, a browser is not the only way to access your application as tools like Postman can still be used to send requests to your application.


For a more robust access control implementation, you will need to write that logic on the server. Though you can do that in a Sails[action](https://blog.sailscasts.com/migrating-your-sails-actions-to-actions2/) , Sails provides you with flexible tools for access control and authorization called **policies** .


## What are policies


Policies provide the ability to execute a reusable set of code before a request executes an[action](https://blog.sailscasts.com/migrating-your-sails-actions-to-actions2/) . Policies can be used as middleware - code that can get in the middle of the request/response cycle. Since Sails policies will be executed before actions, this makes them ideal for managing access to endpoints.


## How policies work


As many part of Sails, the policies implementation is a core Sails[hook](https://sailsjs.com/documentation/concepts/extending-sails/hooks) that has the following responsibilities:


1. Use` sails.modules` to read policies from the user’s app into` self.middleware` .
2. Normalize the policy mapping config (` sails.config.policies` )
3. Listen for` route:typeUnkown` Sails event and bind a policy if the route request it.
4. Listen for router:before and when it fires, transform loaded middleware that match the policy mapping config (i.e. controller actions) to arrays of functions, where the original middleware is “protected” by one or more relevant policy middleware.


## Creating a policy


The most common use-case for policies is to restrict certain endpoints/actions to logged-in users only. Let’s use this use-case as basis to write our first policy. In` policies/` directory create a file and give it the name` isLoggedIn.js` . A Sails policy is a simple exported async function that you pass in the` req` ,` res` and` proceed` arguments. So we will create such a function in` policies/isLoggedIn.js` :


```text
module  .  exports   =   async   function  (  req  ,   res  ,   proceed  ) {
// logic goes in here
}
```


> Note the` proceed` argument is a callback function we call when the request satisfies the policy requirement and can go through.


Let’s now add in the logic to find out if a user is logged in. Do take note that the implementation detail is not focus here so the code will make some assumptions. Here we go:


```text
module  .  exports   =   async   function  (  req  ,   res  ,   proceed  ) {
if   (req.session.userId) {
return   proceed  ();
}


if   (req.wantsJSON) {
// User is not allowed
return   res.  forbidden  (  "You are not permitted to perform this action."  );
}


// Otherwise, this request did not come from a logged-in user.
return   res.  unauthorized  ();
}
```


From the above code block we are making the assumption that when a user is logged in, the session will contain a` userId` property. We check for that property and if we find it, then the user is logged in and the request can go on to the action it was heading for. If we don’t find the userId property however, we will deny the request access to the action.


## Configuring policies


So we have created our first policy let’s get Sails to apply it to some actions.` config/policies.js` which is an ACL(access control list) that exports a dictionary whose keys are the actions and the values are policies for those actions. So let’s say we want every action in our application to be mapped to the` isLoggedIn` policy except the login action(of course we’ve got to let the user log in!). Here is how we do it:


```text
// config/policies.js
module  .  exports  .policies   =   {
'*'  :   'isLoggedIn'  ,
'user/login'  :   true
}
```


The` *` key is a shorthand for targeting every action in our application. While the` true` value for the` user/login` action is a built-in Sails policy that allows a particular action to be access freely.


> Note: policies apply only to actions, not to views. If you define a route in` config/routes.js` pointing to a view, and you want to apply a policy to that route, then you will have to refactor your code to use an action to return that view instead.


## When to use policies


At first glance, we might be thinking to implement a policy anytime we want access control but do avoid implementing numerous or complex policies in your app.


Instead, when implementing features like role-based permissions rely on actions to reject unwanted access. Let’s take for example we want to implement user-level or role-based permissions in your application, it is best to take care of all relevant checks at the top of your action - either in the action itself or calling a helper.


Following this is a best practice in Sails which will significantly improve maintainability of your code.


## Policies ordering and precedence


If you want to apply more than one policy to an action or groups of actions you will need to pass in the policies as an array like so:


```text
'user/*'  : [  'isLoggedIn'  ,   'isEligibleForParityBonus'  ]
```


> Note ‘user/*’ means every action in the controllers/user directory


A good point to note is that the other of the policies matter as Sails will run the first one then go on to the next in succession.


## Built-in policies


Apart from the policies you create in` policies/` , Sails provide two globally applied policies of its own. Let’s take a look at them:


- ` true` : This policy is for public access i.e it allows anyone to access the mapped action(s)
- ` false` : This policy allows no access to the mapped action(s)


## Best practices with policies


-


` '*': true` is the default policy for all actions. In production, it’s good practice to set this to false to prevent access to any logic you might have inadvertently exposed.


-


Don’t access request parameters in policies. A good policy is a reusable policy, looking at request parameters makes a policy to be not reusable. A good policy should only access` req.session` and the database but not request parameters. If you need to look at request parameters then you should write the access control logic in the action it concerns.


-


Policies should be nullipotent this means they should not set, write, or mutate your application state in anyway.


-


Policies should not be a core part of your business logic. Policies are poor tools for structuring logic in your application. Using policies this way makes them dangerous and developer-specific. If you use policies to help with queries or create a complex chain of policies to manage permission systems in your application, you’re likely to create a codebase that’s difficult for you or any other developer to understand.


## Conclusion


Sails provides you with policies as tools to control access to your application endpoints/actions. This article introduced policies, showed you how to write your very first policy and best practices to observe to get the best out of using policies.
