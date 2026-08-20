---
schema_version: "1.0.0"
document_id: "4e7c10b2d61b6bc0b314cbc7f9924d4558eee8e5d03dc270caadd8247815f220"
company_key: "yc-signoz"
company: "SigNoz"
source_id: "yc-signoz-rss-564a62b873f8"
canonical_url: "https://signoz.io/blog/opentelemetry-browser-instrumentation"
published_at: "2026-06-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.602972+00:00"
fetched_at: "2026-07-28T22:07:09.326483+00:00"
content_hash: "sha256:72bd340b157d9b8d68c672d8bb61775b5de262b1b4e9fc7bd539976a2dbe8cd9"
---

# OpenTelemetry Browser Instrumentation Complete Tutorial

# OpenTelemetry Browser Instrumentation Complete Tutorial


Published on: March 20, 2023


Last Updated: June 17, 2026


6 min read


Browser instrumentation refers to collecting and analyzing data about a user's interactions with a web browser. This type of instrumentation involves using specialized tools and techniques to gather information about how a website is being used, such as page load times, network requests, and user interactions.


The data collected through browser instrumentation can be used to improve website performance, identify and troubleshoot errors, and gain insights into user behavior. In this tutorial, we will use OpenTelemetry to instrument a React browser application.


## OpenTelemetry Browser Instrumentation


OpenTelemetry provides libraries that enable the collection of telemetry data from web browsers using the[OpenTelemetry API](https://signoz.io/comparisons/opentelemetry-api-vs-sdk/) . You can collect performance metrics, traces, and other telemetry data from client-side applications running in the browser. The collected data can be exported to an observability backend like[SigNoz](https://signoz.io/) .


By using OpenTelemetry Browser Instrumentation, developers can gain valuable insights into how their web applications are performing and identify opportunities for optimization, leading to a better user experience. This makes it a solid foundation for[frontend monitoring](https://signoz.io/docs/frontend-monitoring/) , including tracking[Nextjs web vitals](https://signoz.io/blog/opentelemetry-nextjs-web-vitals/) and other performance signals. OpenTelemetry is backed by CNCF and is continuously evolving to improve observability for software systems.


OpenTelemetry just provides an instrumentation layer, you would need a backend to store and analyze data. In this tutorial, we will use SigNoz - an open source full-stack observability tool to visualize the collected data.


Let’s learn how to instrument a React browser application with OpenTelemetry.


### Browser Instrumentation with OpenTelemetry


#### Prerequisites


- [Reactjs](https://reactjs.org/)
- SigNoz


#### Install SigNoz


**Step 1: Install SigNoz**


First, you need to install SigNoz so that OpenTelemetry can send the data to it.


SigNoz can be installed on macOS or Linux computers in just three steps by using a simple install script.


The install script automatically installs Docker Engine on Linux. However, on macOS, you must manually install[Docker Engine](https://docs.docker.com/engine/install/) before running the install script.


```text
git clone  -  b main https :  /  /  github .  com  /  SigNoz  /  signoz .  git
cd signoz /  deploy /
.  /  install .  sh


```


You can visit the documentation for instructions on how to install SigNoz using Docker Swarm and Helm Charts.


When you are done installing SigNoz, you can access the UI at` http://localhost:3301/application`


*SigNoz dashboard - It shows services from a sample app that comes bundled with the application*


**Step 2: Get the sample React app**


[Sample React App](https://github.com/SigNoz/react-app-browser-instrumentation) It contains the sample boilerplate code that we will instrument.


```text
git   clone https://github.com/SigNoz/react-app-browser-instrumentation.git


```


#### Instrument React App with OpenTelemetry


**Step 3: Tracing.js file** Our application code consists of a` tracing.js` file. The` tracing.js` file contains the code for setting up OpenTelemetry. You can find the file[here](https://github.com/SigNoz/react-app-browser-instrumentation/blob/main/src/tracing.js) .


**Step 4: Instrument Browser Instrumentation in React app with[OpenTelemetry](https://signoz.io/opentelemetry/)**


To instrument the React app with OpenTelemetry, we need to install the OpenTelemetry dependencies.


```text
npm i @opentelemetry /  api @opentelemetry /  auto -  instrumentations -  web @opentelemetry /  context -  zone @opentelemetry /  exporter -  trace -  otlp -  http @opentelemetry /  instrumentation -  fetch @opentelemetry /  instrumentation -  xml -  http -  request @opentelemetry /  resources @opentelemetry /  sdk -  trace -  web


```


Since we already set up the tracing.js file in the sample react app, you can just change the service name.


*Update the service name to signoz-browser-tracing*


**Step 3: Enable CORS in the OpenTelemetry Collector**


SigNoz installation comes with an OpenTelemetry Collector, which must be configured for receiving traces from the browser application. Enable CORS in the[OTel](https://signoz.io/opentelemetry/) Receiver.


Under SigNoz folder, open the` otel-collector-config.yaml` file. The file is located at` deploy/docker/otel-collector-config.yaml`


You can view the file at[SigNoz GitHub repo](https://github.com/SigNoz/signoz/blob/main/deploy/docker/otel-collector-config.yaml) . Inside the file, add the following CORS config:


```text
http :
cors :
allowed_origins :
-   https :  /  /  netflix .  com    #  URL    of   your  Frontend   application


```


You need to update the URL in the config file to match your frontend application URL. For this tutorial, we will be running our frontend application on` http://localhost:3000` .


```text
http :
cors :
allowed_origins :
-   http :  /  /  localhost :  3000


```


*Enable CORS in SigNoz OTel Collector*


Once you make the changes, you need to restart the Docker containers.


**To stop running the SigNoz cluster:**


```text
cd deploy /  docker
sudo docker compose stop


```


**To start/resume the running SigNoz cluster:**


```text
cd deploy /  docker
sudo docker compose start


```


-


The stopped SigNoz cluster should resume and mount to the existing docker volumes.


**Step 6: Start the React app**


Go to the root folder of your React application, and run the following command:


```text
npm run start


```


Congratulations! You have successfully run your React application with OpenTelemetry. It’s time to see the collected data.


#### Monitor React App with SigNoz


**Step 7: Generate some data**


In order to monitor your React application with SigNoz, you first need to generate some data.


Visit` http://localhost:3000/` to access your frontend application. Using the UI, make some calls to the backend API. You can check the network tab in your browser to see the requests that you have made.


*Network calls for tracing* *Todo list app in React*


**Step 8: Monitor your application with SigNoz**


With SigNoz, you can monitor the data collected by OpenTelemetry from your sample React application. You can see end-to-end traces for your React application.


You can analyze your tracing data with powerful filters using the` Traces` tab on the SigNoz dashboard.


*Browser application in the list of applications monitored in SigNoz*


**Todo 1 got added**


**Todo 2 got added**


**Todo 3 got deleted**


### Conclusion


OpenTelemetry browser instrumentation lets you collect important metrics about performance of browser applications. If you use OpenTelemetry, you also don’t get locked in with any vendor. OpenTelemetry is a one-stop solution for generating and collecting all telemetry signals. You can future-proof your instrumentation by using OpenTelemetry libraries.


OpenTelemetry combined with SigNoz provides a full-stack open source solution. SigNoz provides all three telemetry signals - logs, metrics, and traces under a single pane of glass.


If you have any questions or need any help in setting things up with SigNoz, join our slack community and ping us in` #support` channel.


---


**Further Reading**


[SigNoz - an open source alternative to DataDog](https://signoz.io/blog/open-source-datadog-alternative/)


[Monitor your Express application with OpenTelemetry and SigNoz](https://signoz.io/blog/opentelemetry-express/)
