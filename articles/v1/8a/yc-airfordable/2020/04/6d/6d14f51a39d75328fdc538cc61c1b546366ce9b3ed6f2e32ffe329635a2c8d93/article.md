---
schema_version: "1.0.0"
document_id: "6d14f51a39d75328fdc538cc61c1b546366ce9b3ed6f2e32ffe329635a2c8d93"
company_key: "yc-airfordable"
company: "Airfordable"
source_id: "yc-airfordable-rss-2050107362de"
canonical_url: "https://medium.com/airfordable-engineering/uptime-robot-datadog-alert-integration-ef6a45442d00"
published_at: "2020-04-17T21:54:59+00:00"
first_seen_at: "2026-08-09T18:52:50.899160+00:00"
fetched_at: "2026-08-20T02:43:01.710414+00:00"
content_hash: "sha256:fa2ff95dd11a1d3c59c7533db57ddb1d93922945c32c6dcecc3efaf72424a09f"
---

# Uptime Robot + DataDog Alert Integration

### Uptime Robot + DataDog Integration


At[Airfordable](https://www.airfordable.com/) we employ Uptime Robot as one of the services responsible for monitor our sites. Per dollar spent, Uptime Robot is a very reasonable tool, but while it is a good value, they have not yet managed to create easy integrations with all of the cloud based monitoring tools in the market. DataDog is once such monitoring provider that lacks a direct integration with Uptime Robot. As a result, we had to manually integrate Uptime Robot with DataDog using DataDog APIs and Uptime Robot web-hooks. This blog post will cover how we did it and what you need to know to make it happen.


### DataDog


For those who are unfamiliar, DataDog is a cloud service that allows you to monitor your infrastructure using tools they provide and is also capable of ingesting real-time metrics via a StatsD like protocol. They also do a lot more like log ingestion, but that is beyond the scope of this post.


At[Airfordable](https://www.airfordable.com/) we use DataDog to monitor all of our infrastructure and centralize all alerts and actions. For this solution, we want DataDog to receive events as they happen from Uptime Robot so we can monitor all important happenings in a central location. These events allow us to not only view what is happening, but also trigger alerts and actions based on event frequency.


DataDog has a nice set of HTTP APIs that allow us to interact with their system, and for this project we will use this event API. To create an event using the[DataDog Event API](https://docs.datadoghq.com/api/?lang=bash#post-an-event) , we need to POST a properly formatted JSON object to the /v1/events endpoint. Here is an example from their page of what the JSON blob might look like:


```text
{   “title”: “Did you hear the news today?”,   “text”: “Oh boy!”,   “priority”: “normal”,   “tags”: [“environment:test”],   “alert_type”: “info”  }
```


But before we can do anything with the API, we first must create an API key. This can be done under the[DataDog API Keys drop down](https://app.datadoghq.com/account/settings#api)


With our API key in hand and a basic understanding of what DataDog needs in an event API request, we are ready to move on.


### Uptime Robot Web-Hooks


Uptime Robot is cloud service whose purpose is to check that your website or service is up and available. It does this by making a request or opening a new connection to an endpoint you specify on a regular basis. It can also listen for a heartbeat from a service and alert if the heartbeat stops. Since we use Uptime Robot to ensure our site is up, we want to be notified if/when our site goes down (which of course it never does…).


Uptime Robot has a few ways of notifying you when one of your services goes down. You can receive a call, receive a text message, get an email, aSlack alert, or have them execute a web-hook. The web-hook alert is a great solution for us here since it will allow us to make a call to an arbitrary API endpoint that accepts POST requests in a JSON format. This is perfect because this is exactly what the DataDog API does.


For the web-hook to be effective and interact properly with DataDog we have to format the web hook in a way the DataDog API will be able to parse and understand. This can be done by creating a JSON blob and employing the template system that Uptime Robot has implemented. The system works by replacing the template strings with corresponding values related to the monitor in Uptime Robot that is alerting. The strings we will make use of are noted as follows:


With these template strings in mind, and the ability to create and send an arbitrarily formatted JSON object, we are now ready to tie everything together.


### Tying it all Together


Below is an example JSON object with template strings that we could send to the DataDog event API:


```text
{   “title”:”Uptime_Robot: *monitorFriendlyName* is *alertTypeFriendlyName*”,   “text”:”*monitorFriendlyName* is *alertTypeFriendlyName*: *alertDetails*”,   “host”:”*monitorURL*”,   “aggregation_key”:”*monitorID*-*alertType*”,   “alert_type”: “warning”  }
```


As previously noted, Uptime Robot will substitute the strings surrounded with ‘*’ characters with values noted in the table above. This allows the alert to work with all of our monitors. A few note about the above JSON blob:


- We set the aggregation_key to the *monitorID*-*alertType* so all similar events would be grouped together in the DataDog event stream. You can remove the -*alertType* suffix if you want to group up and down events together.
- You may want to set some tags to make categorization easier in DataDog.


With this template JSON blob created, we now just need to figure out where to send it. As we noted above the The DataDog API endpoint is hosted at[https://api.datadoghq.com/api/](https://api.datadoghq.com/api/) . We just need to append the path we want (/v1/events) and include any required HTTP query parameters (api_key). A full exampleURL might look something like this:


[https://api.datadoghq.com/api/v1/events?api_key=A1B2C3D4E5F6G7H8I9K0](https://api.datadoghq.com/api/v1/events?api_key=A1B2C3D4E5F6G7H8I9K0)


With our JSON template object ready and API URL generated, we are ready to create the alert contact in Uptime Robot.


### Creating the Alert Contact in Uptime Robot


Login to Uptime Robot and do the following:


- Navigate to[My Settings](https://uptimerobot.com/dashboard#mySettings)
- Select the “Add Alert Contact” button


Once you do that you should see a screen like this:


- In the “Alert Contact Type” field, select “Webhook”.
- In the “Friendly Name” field enter a name of your choice.
- In the “URL to Notify” field enter the URL created in the last section. Uptime Robot requires the string to end with a ? or & character so they can append values. Go ahead and add a trailing & character to the URL we created above.
- In the “POST Value” field enter the JSON blob from the last section.
- Ensure the “Send as JSON” checkbox is selected.
- In the “Enable notification for” field select “Up & down events”.
- Click “Create Alert Contact” button at the bottom of the window.


With that done you should now be able to add this alert to any monitor you have and get a corresponding event in DataDog.


### Wrapping Up


After doing this it would be best that you associate the alert with a test monitor and cause the monitor to fail by mis-configuring it. This should send the alert to DataDog and you can verify everything works as expected.


Ideally DataDog would have a direct integration with Uptime Robot to save us the trouble here, but for now this is good enough.


---


[Uptime Robot + DataDog Alert Integration](https://medium.com/airfordable-engineering/uptime-robot-datadog-alert-integration-ef6a45442d00) was originally published in[Airfordable Engineering](https://medium.com/airfordable-engineering) on Medium, where people are continuing the conversation by highlighting and responding to this story.
