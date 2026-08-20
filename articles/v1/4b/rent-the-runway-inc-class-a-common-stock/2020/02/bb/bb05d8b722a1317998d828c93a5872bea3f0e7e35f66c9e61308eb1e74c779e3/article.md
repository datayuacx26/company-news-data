---
schema_version: "1.0.0"
document_id: "bb05d8b722a1317998d828c93a5872bea3f0e7e35f66c9e61308eb1e74c779e3"
company_key: "rent-the-runway-inc-class-a-common-stock"
company: "Rent the Runway Inc."
source_id: "rent-the-runway-inc-class-a-common-stock-rss-571d2b3d11b7"
canonical_url: "https://dresscode.renttherunway.com/blog/rtr-react-okta-auth"
published_at: "2020-02-26T17:12:01+00:00"
first_seen_at: "2026-07-20T23:19:27.389009+00:00"
fetched_at: "2026-07-28T21:05:25.747778+00:00"
content_hash: "sha256:c669fe58cd5e9e3edd78e745b101ed9c76f60e3eb357823c6abf0f310895b75b"
---

# Don't React Now: You're Not Authorized; Taking Stock of Okta Auth

## @rent-the-runway/rtr-react-okta-auth


Okta is an excellent facility for both internal and external user management. It’s complete with a flexible administration facility and a comprehensive Application Programming Interface (API).


Here at Rent the Runway, we leverage Okta to provide standardization across some internal systems. I am a front end developer on one of those systems.


To that end, finding[@okta/okta-react](https://www.npmjs.com/package/@okta/okta-react) was an exciting prospect; the documentation is great and it allows a React application to effortlessly integrate with Okta. Adding Okta authentication to our application became a frictionless task. However, our requirements went a little beyond the offerings from this library. We needed more than just authentication, we wanted to permit access only if a user belonged to a particular Okta user group.


Introducing[@rent-the-runway/rtr-react-okta-auth](https://github.com/RentTheRunway/rtr-react-okta-auth) , an open source NPM library to facilitate Okta integration for ReactJS applications. It’s a layer of abstraction built on top of[@okta/okta-react](https://github.com/okta/okta-oidc-js/tree/master/packages/okta-react) .


Authentication informs us if the user is logged in or not. Authorization informs us if a logged in user has access to a particular resource.


This is the difference between[@okta/okta-react](https://github.com/okta/okta-oidc-js/tree/master/packages/okta-react) and[@rent-the-runway/rtr-react-okta-auth](https://github.com/RentTheRunway/rtr-react-okta-auth) .


[@okta/okta-react](https://github.com/okta/okta-oidc-js/tree/master/packages/okta-react) facilitates authentication;[@rent-the-runway/rtr-react-okta-auth](https://github.com/RentTheRunway/rtr-react-okta-auth) facilitates authorization.


Meanwhile, another development team was also investigating how they might integrate Okta into their application. Their requirements were a little different again. They needed to permit access based on permissions. For them, knowing the Okta user groups was not enough. They wanted to associate a list of permissions with each Okta user group. This allows a more granular and flexible level of control. Their requirements were for Role Based Access Control (RBAC).


It quickly became apparent that the solution to all this was to build an Okta integration library for the business rather than the individual applications. Once we built it, the next obvious step was to open source it for the benefit of all.


Authorization can be achieved based off of Okta user groups or Okta claims. In addition to this, we facilitate a more generic approach to authorization. For example, whereas Okta does not natively facilitate RBAC, our generic approach allows the application to implement RBAC by having application-level permissions associated with Okta user groups.


Note that both[@okta/okta-react](https://github.com/okta/okta-oidc-js/tree/master/packages/okta-react) and[@rent-the-runway/rtr-react-okta-auth](https://github.com/RentTheRunway/rtr-react-okta-auth) are concerned primarily with the React application but not limited to the React application. Server-side authentication and authorization can be addressed independently as per the Okta documentation.


## COMPONENT LIBRARY


Here’s an example of some of the components available in the library.


## LOCKING OUT JSX


```text
<WhenMemberOfAny groups={["standard", "admin"]}>
```


```text
<li className="nav-item">
```


```text
<Link to="/orders" >
```


```text
Orders
```


```text
</Link>
```


```text
</li>
```


```text
</WhenMemberOfAny>
```


This component will simply not render its ‘children’ if the user is not authenticated or is not a member of Okta user groups ‘standard’ or ‘admin’.


## LOCKING OUT ROUTES


```text
<RouteWhenMemberOfAny
```


```text
groups={["standard", "admin"]}
```


```text
path="/orders"
```


```text
exact={true}
```


```text
component={Orders}
```


```text
/>
```


This component will prevent access to the Route (react-router-dom) unless the user is authenticated and is a member of Okta user groups ‘standard’ or ‘admin’. If unauthenticated, the browser will redirect to the Okta login screen. Upon successful authentication, the client application will receive the Okta groups and will permit or deny access to the Route accordingly.


## AUTHENTICATION STATE AWARE


The library works off the basis that the application is not rendered until after Okta has been challenged and the authentication state of the user is known. Only then will the application render in earnest.


The authentication state includes all groups and claims the user is associated with. The components provided by the library leverage this information to lock out Routes and UI fragments accordingly. All this authentication state information is available to the client application via the Context API.


For a full list of components and features and how to setup Okta accordingly, see the comprehensive documentation on[github](https://github.com/RentTheRunway/rtr-react-okta-auth) .


## OPEN SOURCING


As an organization, we use a whole host of open source libraries and solutions. Given that this particular code is of benefit to us as an organization and is nonproprietary, we elected to give back to the community and open source it for the benefit of all. We recognize the value of open source, and we delight in our continued contributions which you can checkout on[github](https://github.com/RentTheRunway) . Here are a few highlights:


-


[Alchemy](https://github.com/RentTheRunway/alchemy) is a framework for running A/B experiments — fast and ops-friendly!


-


[Conduit](https://github.com/RentTheRunway/conduit) is a simple abstraction over the RabbitMQ Java client, which hides most of the AMQP/RabbitMQ specific details.


-


[SwiftyJSONAPI](https://github.com/RentTheRunway/SwiftyJSONAPI) is a Swift framework for parsing JSONAPI documents.


## WE VALUE THIS SOLUTION


Rent the Runway has a rich and vibrant culture; it’s a great community to be a part of. Once we floated the idea of an open source library, our tech leadership facilitated and encouraged us all the way from inception through to the end result. Also in keeping with the culture of the company, the quality of the library is very high with over 98% test coverage.


For more on test and quality, check out Nicole Weber’s awesome blog post on “[Leveraging flaky tests to identify race conditions using cypress](http://dresscode.renttherunway.com/blog/leveraging-flaky-tests) .”


Watch this space for more contributions from Rent the Runway.
