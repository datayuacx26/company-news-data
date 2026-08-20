---
schema_version: "1.0.0"
document_id: "c5dad2ec302ceacb4d18dd175ed8688ce08a21378beb8dfd6542ae7b03a7cc65"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/oauth2-login-python-flask-apps"
published_at: "2023-11-21T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:01:37.179147+00:00"
content_hash: "sha256:aecb65c5e656c1c9bd46e777f093c8e35413ccda852fd240b3753c867524f4d6"
---

# GitHub OAuth in your Python Flask app

In this guide we'll learn how to quickly build an OAuth2.0 integration into a simple Flask app using Supabase-py. This will enable your users to login to your web app using their GitHub account.


## Prerequisites#


This article assumes you are familiar with creating an application in Flask. It also assumes that you have read the Supabase documentation and are familiar the with concept of[Authentication](https://supabase.com/docs/guides/auth) .


We'll use the following tools:


- [Flask](https://flask.palletsprojects.com/en/3.0.x/) - we used version 2.3.3 for this article
- Supabase Dashboard -[create an account](https://database.new/) if you don't have one already


## Getting started#


To begin, inside your Flask application install the` supabase` library using the following command in the terminal:


`
_10


pip install supabase


`


## Session storage#


Open the project in your preferred code editor and create a file called` flask_storage.py` with the following content:


`
_17


from gotrue import SyncSupportedStorage


_17


from flask import session


_17


_17


class FlaskSessionStorage(SyncSupportedStorage):


_17


def __init__(self):


_17


self.storage = session


_17


_17


def get_item(self, key: str) -> str | None:


_17


if key in self.storage:


_17


return self.storage\[key\]


_17


_17


def set_item(self, key: str, value: str) -> None:


_17


self.storage\[key\] = value


_17


_17


def remove_item(self, key: str) -> None:


_17


if key in self.storage:


_17


self.storage.pop(key, None)


`


In this file, we're extending the` SyncSupportedStorage` class from the` gotrue` library which comes bundled with the` supabase` library. Here we're telling the Supabase authentication library (` gotrue` ) how to retrieve, store and remove a session that will store our JSON Web Token (JWT).


## Initiate the client#


Create another file called` supabase_client.py` and in this file, we'll initiate our Supabase client.


`
_22


import os


_22


from flask import g


_22


from werkzeug.local import LocalProxy


_22


from supabase.client import Client, ClientOptions


_22


from flask_storage import FlaskSessionStorage


_22


_22


url = os.environ.get("SUPABASE_URL", "")


_22


key = os.environ.get("SUPABASE_KEY", "")


_22


_22


def get_supabase() -> Client:


_22


if "supabase" not in g:


_22


g.supabase = Client(


_22


url,


_22


key,


_22


options=ClientOptions(


_22


storage=FlaskSessionStorage(),


_22


flow_type="pkce"


_22


),


_22


)


_22


return g.supabase


_22


_22


supabase: Client = LocalProxy(get_supabase)


`


Let's focus on the` get_supabase` function. Here we are checking if we have an instance of the client stored in our global object` g` , if not we create the client and store it in the global object under the` supabase` name. You will notice in the` ClientOptions` that we are specifying the` FlaskSessionStorage` class we created earlier and we are also specifying a very important option that allows us to handle the OAuth flow on the server side, the` flow_type="pkce"` .


## Sign in with GitHub#


Supabase Auth supports Sign in with GitHub on the web, native Android applications, and Chrome extensions.


For detailed set up and implementation instructions please refer to the[docs](https://supabase.com/docs/guides/auth/social-login/auth-github) .


## Create sign-in route#


Inside our application code` app.py` , we can create the sign-in route to trigger the OAuth sign-in request.


`
_11


@app.route("/signin/github")


_11


def signin_with_github():


_11


res = supabase.auth.sign_in_with_oauth(


_11


{


_11


"provider": "github",


_11


"options": {


_11


"redirect_to": f"{request.host_url}callback"


_11


},


_11


}


_11


)


_11


return redirect(res.url)


`


In this function` options` object we specify a` redirect_to` parameter which will point to the callback route we will create in the next step. This function will generate a url for us to use to redirect the user to, in this case we are using` github` as our OAuth provider so we will be redirected to the GitHub OAuth consent screen.


## Create callback route#


Let's add another route to our` app.py` file for the callback endpoint we specified in our sign in route.


`
_10


@app.route("/callback")


_10


def callback():


_10


code = request.args.get("code")


_10


next = request.args.get("next", "/")


_10


_10


if code:


_10


res = supabase.auth.exchange_code_for_session({"auth_code": code})


_10


_10


return redirect(next)


`


Here we're getting the` code` query parameter from the request object, if this is available we then exchange the code for a session so that the user will be signed in. Under the hood the` supabase` python library will handle storing this session (JWT) into a cookie and sign the user in.


## Conclusion#


In this post we explained how to setup a flask session storage to work with the Supabase python library, setting the` flow_type` to use Proof Key for Code Exchange (PKCE) and creating a sign in and a callback route to handle the user authentication.


## More Resources#


- [supabase-py reference docs](https://supabase.com/docs/reference/python/installing)
- [supabase-py GitHub repo](https://github.com/supabase-community/supabase-py)
- [Deep Dive series on auth concepts in Supabase](https://supabase.com/docs/learn/auth-deep-dive/auth-deep-dive-jwts)
