---
schema_version: "1.0.0"
document_id: "d3fac334b76b90d89bad18fcd43a64fcb8422ce2d2c8c7472440d15b76e920c1"
company_key: "yc-adaptyv"
company: "Adaptyv"
source_id: "yc-adaptyv-news-import-3c8571c35bcb"
canonical_url: "https://www.adaptyvbio.com/blog/automancer"
published_at: "2023-07-11T00:00:00+00:00"
first_seen_at: "2026-07-24T14:21:15.816567+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:bccc7a5f783b78f5bdf9a29ae44711bba0d5f55accb76cd5e357c5a31b97358e"
---

# Automancer: open-source, flexible lab automation software

Despite being the key to accelerating life science research in academia and industry, lab automation has been adopted far too slowly. Without programming expertise or access to expensive commercial solutions, it remains difficult for startups or labs to develop automation that satisfies their requirements of integrating various different components in experimental setups.


Here, we present


[Automancer](https://automancer.adaptyvbio.com/)


, a software application that enables researchers to design, automate, and manage their experiments. Automancer is open-source, easy-to-use, and can automate just about any process in your lab.


#### **How does Automancer work?**


- Connect your devices (via a configuration file)


- Design your protocol and visualize it in a workflow diagram


- Click run and monitor the experiment in real-time (or go read the latest research paper)


- Get notified when the experiment has finished and log the results in a report


Protocols typically interact with lab devices, such as temperature controllers, relays, microscopes and liquid handlers, but can perform any action, such as uploading a file to S3 or asking the user for confirmation. All of those functionalities are defined via plugins. Automancer is highly extensible, you can support custom actions by creating your own plugins and developing your own connections for devices.


Protocols are written in a simple, YAML-like declarative text language. In the example below, the protocol acts on a setup which only contains a camera. Automancer will first ask for the user to confirm that the camera’s focus is ready, and then create a timelapse by taking a picture every 10 seconds, until 200 pictures have been taken.


```text
name: Timelapse
protocol:
actions:
- query: Confirm that focus is correct # Sends a prompt to the user
- actions:
- Camera.capture:
exposure: 50 ms
output: {{ f"images/{index}.jpg" }}
- wait: 10 sec
repeat: 200
```


Once running, protocols can be interacted with at any moment. They can be paused, halted, modified, etc. Automancer will take appropriate action to ensure graceful resource management, e.g. by closing open files, stopping timers or turning off unused devices.


For more advanced protocols, Automancer has support for expressions that depend on variables that change in real-time (like sensor feedback). In this second example, the setup being controlled contains a temperature controller which must reach its target temperature before proceeding. To implement this, we use the ‘


***until’***


clause with a check against the temperature readout.


```text
name: Temperature control
protocol:
TempController.setpoint: 37 degC
actions:
- until: {{ TempController.readout > 36.5 * unit.degC }}
- ... # Proceed with the rest of the protocol
```


Note how achieving a similar result using a regular programming language would require a much larger amount of code, and be significantly less readable, as it would need to periodically check the temperature in a loop. Automancer abstracts away this logic to allow the scientist to focus on experiment design.


Let’s take a look at how this looks like in practice!


#### Example 1: Control reagent flows


In this protocol we want to flow reagents onto a microfluidic chip by controlling both external rotary valves and on-chip microfluidic valves. Automancer makes it easy to integrate the different devices and control them via one protocol.


#### Example 2: Optimize droplet generation


Here we want to generate a stable microfluidic droplet stream by controlling the pressure of the droplet phase vs the pressure of the encapsulation phase. Using Automancer, we can easily write a function that decreases the droplet phase pressure periodically and then uses a microscope camera to record images of the droplet stream, identify individual droplets and measure their size.


#### **Features**


- **Modularity**


– Apart from the core functionality and UI, most of Automancer’s capabilities are provided through plugins. Plugins are Python modules that can extend Automancer in one or more ways: they can add support for a new device, provide a custom protocol logic feature, report information to the UI… pretty much whatever you can dream up


- **User interface**


– Automancer’s user interface provides easy control to users with no coding expertise. Furthermore, by supporting plugin development, we make extending the user interface simple, in particular when employing the built-in UI components . For instance, this can be used to report data generated through Python, allowing Python developers without a background in UI development to customize their view of the application.


- **Protocol format**


– The declarative and human-readable (YAML-like) text format used by Automancer has numerous benefits. Unlike proprietary formats, text files are lightweight, easy to share, to reuse and to compare between iterations (we simply use Git for versioning). The language can be learned quickly by beginners and does not require any coding expertise. Experienced users can extend the language using plugins. Automancer provides an optional built-in editor with advanced programmatic language features, such as live completion, errors and documentation on hover.


- **Remote control**


– Experiments can be started and controlled from another computer on the same or another network connected to the Internet. This allows multiple people in different locations to monitor the same experiments, or when running experiments on multiple setups, to control them all from the same computer.


- **Written in Python and open source**


– Automancer is written in Python for the backend and TypeScript/SCSS with React for the user interface, running on Electron. It is available under the MIT license and open to contributions.


At Adaptyv Bio, Automancer allows us to explore more versions of experiments (i.e. protocol variants) either for assay development (searching for an assay configuration that works) or for process optimization (searching for what works best). It also helps us to limit human intervention and error, improve reproducibility, and run experiments out of office hours. Thanks to the text nature of protocols, we can store these in a Git repository to share them among members of the team and keep track of changes from one experiment to another. Development of Automancer was originally born out of a collaboration with the


[Maerkl Lab at EPFL](http://lbnc.epfl.ch/)


and we would like to thank the lab for their support.


If you have any questions, suggestions, or would like to contribute to the project, please feel free to reach out to us at


automancer@adaptyvbio.com


or check out the code & docs.


#### Resources


- [Automancer Github repo](https://github.com/adaptyvbio/automancer)


- [Automancer documentation](https://automancer.adaptyvbio.com/)
