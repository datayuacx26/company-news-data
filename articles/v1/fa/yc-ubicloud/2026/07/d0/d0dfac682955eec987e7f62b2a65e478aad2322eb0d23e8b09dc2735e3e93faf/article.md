---
schema_version: "1.0.0"
document_id: "d0dfac682955eec987e7f62b2a65e478aad2322eb0d23e8b09dc2735e3e93faf"
company_key: "yc-ubicloud"
company: "Ubicloud"
source_id: "yc-ubicloud-news-import-c10303752e5c"
canonical_url: "https://www.ubicloud.com/blog/ubiclouds-thin-client-approach-to-command-line-interfaces"
published_at: null
first_seen_at: "2026-07-24T05:09:51.579811+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:eadc1078a86637cfaca366389b50fb0add4bcc623cfe72bed775dcde956611ed"
---

# Ubicloud's Thin CLIent approach to command line interfaces

## Ubicloud's Thin CLIent approach to command line interfaces


April 2, 2025 · 4 min read


Jeremy Evans


Principal Software Engineer


Ubicloud has supported both a web and API interface since launch. Recently, we decided to also support a command line interface, so that users can interact with Ubicloud directly from their terminal. To implement the command line interface, we took inspiration from old computer terminals. Everything old is new again.


### The Traditional Approach


Traditionally, command line interface programs that interact with web services work like this. The program parses the arguments given by the user, determines the API endpoints, and then submits requests to those endpoints. The program then takes the responses from those endpoints and formats them for display to the user.


We’ll be using the following terms in this post:


- client: the CLI program the user runs.
- target: The API endpoint that returns the requested information or makes the requested changes.


The traditional approach has two main problems. First, every new feature added to the client requires that users update their client in order to use the feature. This gives up the main advantage of web distribution, which is that users can immediately benefit from new features and bug fixes as soon as you put them into production. Second, it is harder to detect bugs in the command line interface, since bugs will generally occur in the client, on the user's machine, and you have to hope the user reports them.


Let's see how we mitigate these issues with the Thin CLIent approach.


### The Thin CLIent Approach


With the Thin CLIent approach, the client does no parsing of the arguments given by the user. Instead, it just sends all arguments to a single endpoint, which we’ll call the gateway. The gateway endpoint handles requests and responses in the following way:


- It parses the arguments provided by the client, determines the appropriate target endpoints to contact, and the request parameters for those endpoints. It then submits internal API requests to the target endpoints.
- It handles the responses from the target endpoints, and formats them for display to the user. It then provides the formatted plain text response as the response body to the client. The client prints the body of the response directly to the terminal.


Using the Thin CLIent approach offers the following advantages:


- New features can be added to the command line interface without the user needing to update their client. This brings the advantages of web distribution to command line interfaces.
- The client can be much smaller in terms of code/implementation size than in the traditional approach, as most of the complexity is moved from the client to the gateway endpoint. The overall complexity is about the same, but complexity in the gateway endpoint is easier to manage than client complexity, as it runs in an environment controlled by Ubicloud.
- Bugs in the command line interface will generally occur on the gateway endpoint and not in the client. Since the gateway endpoint runs in an environment controlled by Ubicloud, it’s easier to identify and fix such bugs.
- The vast majority of logic is executed on the gateway endpoint and can be implemented in whatever language you prefer. Ubicloud is implemented in Ruby and we would prefer to use Ruby when we have the option to do so.
- Some commands require contacting multiple target endpoints. The most common example is if you need information from somewhere before making a request to a target endpoint. For example, if we are requesting a restart of a virtual machine given only the machine’s id, we first need to contact a directory node to get the location of the virtual machine to restart. The Thin CLIent approach makes this faster, as internal requests from the gateway endpoint to the target endpoints have no network latency, so you only pay the latency cost for the single request from the client to the gateway endpoint.


The Thin CLIent approach has some disadvantages compared to the traditional approach:


- All commands require contacting the gateway endpoint, so even getting help output takes some time.
- Commands that require user input will need multiple requests to the gateway endpoint. A simple example is when the gateway endpoint requests confirmation from the user to permanently delete cloud resources. With Thin CLIents, this requires an extra hop between the user and the gateway.


We believe the advantages of the Thin CLIent approach heavily outweigh the disadvantages.


### Securely Handling Program Execution


When using a Thin CLIent approach for a client that executes other programs, you have to take additional care in regards to security. For example, if the gateway endpoint is compromised and instructs the client to run rm -rf /


, you do not want the client to execute that. To mitigate the damage a rogue gateway endpoint can inflict on a client, you need to limit the programs the client is allowed to execute, and limit the arguments to those programs.


Ubicloud's client (named ubi


), can only execute the following programs:


- ssh


: to connect to a virtual machine via ssh


- sftp


: to connect to a virtual machine via sftp


- scp


: to copy a file/directory from the local computer to a virtual machine or from a virtual machine to the local compute via scp


- psql


: to connect to a PostgreSQL database via psql


- pg_dump


: to dump a single PostgreSQL database using pg_dump


- pg_dumpall


: to dump an entire PostgreSQL database cluster using pg_dumpall


ubi


will refuse to execute a program that is not one of these 6 programs. Additionally, ubi


requires that the program name be one of the arguments it was passed (preventing the server from requesting execution of a different program).


Additionally, ubi


enforces the following restrictions on the arguments:


- The arguments must include --


to separate arguments from options (except for pg_dumpall


, which does not support this).
- The arguments must include at most one new argument not in the arguments passed to ubi


- The new argument must come after --


(for non- pg_dumpall


) or must start with -d


(for pg_dumpall


)


These restrictions are sufficient to allow the 6 commands above to work safely. It's possible we will need to adjust these restrictions if we support additional programs in the future. The goal will remain to limit the flexibility when executing programs to the minimum needed for the client to function.


### Command Line Program Configuration


Using command line options or arguments for configuration of the client is not possible with the Thin CLIent approach, since the client does not parse them, it only sends them to the gateway endpoint. So you cannot use an approach similar to ubi --gateway https://api.example.com/cli …


to specify a custom endpoint. For ubi


, we decided to have all program configuration be through environment variables. It would be possible to have the client look for configuration in a file at a known location, but the only required configuration option is the token used for the request (which should be secret), and security-conscious organizations are probably not going to want to store secrets in files on local machines.


There are many different systems that handle secret storage, and it's infeasible for ubi


to attempt to integrate with all of them. So instead, ubi


only deals with environment variables, and users can use whatever secret storage approach they want, as long as they call ubi


with the appropriate environment variables set.


### Command Line Program Development


As Ubicloud is written mostly in Ruby, it should come as no surprise that ubi


was prototyped in Ruby. However, we do not want to require that our users have Ruby installed in order to use ubi


. From our initial planning stages, our goal was to ship dependency-free native clients for popular operating systems, so that users could run \`ubi\` without installing anything else.


We ultimately decided to port the Ruby version to Go. While not as programmer friendly as Ruby, Go is still relatively easy to work with, our engineering team has experience with it, and importantly, it allows us to easily ship native clients for macOS, Linux, and Windows.


Next up


[Quick Start - Build your own cloud](https://www.ubicloud.com/docs/quick-start/build-your-own-cloud)


### Feedback Requested


As our command line interface is new, we are eager to receive feedback regarding it. If you are interested, please review the documentation ([https://www.ubicloud.com/docs/quick-start/cli](https://www.ubicloud.com/docs/quick-start/cli) ) and try it out. You can then provide feedback at[\[email protected\]](https://www.ubicloud.com/cdn-cgi/l/email-protection#285b5d5858475a5c685d4a414b44475d4c064b4745175b5d4a424d4b5c157d4a414b44475d4c0d1a186b64610d1a184e4d4d4c4a494b43) or by opening a[discussion on GitHub.](https://github.com/ubicloud/ubicloud/discussions)
