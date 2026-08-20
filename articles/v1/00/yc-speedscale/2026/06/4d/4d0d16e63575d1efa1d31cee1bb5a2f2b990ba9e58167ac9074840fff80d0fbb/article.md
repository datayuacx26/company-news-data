---
schema_version: "1.0.0"
document_id: "4d0d16e63575d1efa1d31cee1bb5a2f2b990ba9e58167ac9074840fff80d0fbb"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/fix-403-auth-errors-traffic-replay-proxymock/"
published_at: "2026-06-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:35893f3542f388c83d5ca1c76e7aa0269ef5ba42f0a37e14ea8faa2590ddef8a"
---

# Fixing 403 auth errors when you replay traffic

Trigger warning: this one is about Java, authentication, and Docker Compose files. If that is not your thing, I am sorry, but they are part of life and they are honestly not that hard to work with. Everything here is open source on our[GitHub repo](https://github.com/speedscale/demo/tree/master/java-auth) , so you can follow along.


*Recording an authenticated Java flow, replaying it, hitting the dreaded 403, and fixing it with a proxymock recommendation.*


## A little demo app that logs in and calls some APIs


The demo is a Spring Boot service that hands out JWT bearer tokens. A test script logs in once and then makes a few authenticated calls with the token it got back:


```text
./scripts/test.sh   localhost:8081
```


It logs in, grabs the` accessToken` , and calls` GET /api/auth/user` three times with` Authorization: Bearer <token>` . No sweat. That works.


What I actually want is more visibility into what these calls do, and a way to turn them into repeatable tests.


## Recording the traffic with proxymock


[proxymock](https://proxymock.io/) is a free tool that records the traffic going in and out of your app. You run the recorder, tell it the port your app listens on, and point your client at proxymock’s port (` 4143` ) instead:


```text
proxymock   record   --app-port   8081   --   docker   compose   up
```


Run the same test script through port` 4143` , and now I have a recording. proxymock writes one Markdown file per request. If you use AI coding tools, they like reading those directly. If you want to read them as a human, there is a web interface:


```text
proxymock   web
```


I can see the login call and the three user calls, each with its request, response, and headers.


## Replaying it, and the dreaded 403


What I really want is to replay this traffic so I have some test automation around the code. So I run it back against my local app:


```text
proxymock   replay   --in   ./proxymock   --test-against   localhost:8081
```


Except this did not work right. The first call worked, but the rest did not. We got the dreaded` 403` .


If you have ever tried to diagnose one of these, you know the pain. You look in the response and there is nothing there. You look in the headers and they do not tell you what is going on.


The information is here, you are just not allowed to see that data. And the reason is we are replaying with the old token. The recording captured the bearer token that was minted at record time, that token has since expired, and the replay is faithfully resending it on every call after the login.


## Reviewing the recommendations


So to fix that, you come into the **Recommendations** panel. proxymock already analyzed the replay, and there is a recommendation that identifies the OAuth handshake. It found the access token and highlighted it. Accept that one.


There are other recommendations too. proxymock noticed an email address in the traffic, for instance. I happen to like that email and want it to pass through unchanged, so I leave that recommendation alone.


Now run the replay one more time:


```text
proxymock   replay   --in   ./proxymock   --test-against   localhost:8081
```


This time the console says` Applied 1 active blueprint(s) to replay` , and I get a 100% match rate. Everything works, because the recommendation made sure the token the app issues on the first call is the token used on all the rest.


## What the recommendation actually did


If you are curious, proxymock saved the fix as a **blueprint** you can open and inspect. Here is the chain it built:


```text
(networkaddr CONTAINS "localhost"
AND location IS "/api/auth/session/login"
AND command IS "POST")
| res_body()
| json_path(path=accessToken)
| smart_replace_recorded(overwrite=true)
```


It goes into the response body of the login endpoint, uses a JSON path to grab the` accessToken` , and runs a smart replace. Because the smart replace matches on the token value, it rewrites the` Authorization` header on every protected request without me naming each one.


There is a whole library of these transforms if you want to do more, but the recommendations are a quick and dirty way to get up and running. This is the same token correlation our cloud product does, running locally on your laptop.


## Try it yourself


The full walkthrough lives in the[proxymock docs](https://docs.speedscale.com/proxymock/guides/recommendations/) , and the demo app is[on GitHub](https://github.com/speedscale/demo/tree/master/java-auth) . There is a lot more you can do with the recorded data. If you have questions, reach out at[proxymock.io](https://proxymock.io/) .
