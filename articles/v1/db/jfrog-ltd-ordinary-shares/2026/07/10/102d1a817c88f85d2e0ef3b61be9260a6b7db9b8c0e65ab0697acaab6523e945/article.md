---
schema_version: "1.0.0"
document_id: "102d1a817c88f85d2e0ef3b61be9260a6b7db9b8c0e65ab0697acaab6523e945"
company_key: "jfrog-ltd-ordinary-shares"
company: "JFrog Ltd."
source_id: "jfrog-ltd-ordinary-shares-rss-4486f0fc66d1"
canonical_url: "https://jfrog.com/blog/using-jfrog-cli-without-editing-ci-scripts/"
published_at: "2026-07-22T13:16:19+00:00"
first_seen_at: "2026-07-22T13:49:19.694403+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:3fa9f1f58e3e1f7eb7e57a1efc44dba532d414cd067b849150a31d45947e778d"
---

# How to Use JFrog CLI Without Editing Your CI Scripts

Using[JFrog CLI](https://docs.jfrog.com/integrations/docs/jfrog-cli) used to come with a catch. To run a build through it, you had to put it in front of the command. For example,` jf npm install` instead of` npm install` or` jf mvn package` instead of` mvn package` per the example below:


` npm install` →` jf npm install`
` mvn package` →` jf mvn package`


Doing that once is easy enough. But across dozens of pipelines, and again with every new project, it adds up to CI scripts you have to keep editing, which becomes yet another responsibility of the overtasked DevOps team.


**JFrog CLI has now done away with the**` jf` **prefix** . By setting the parameter:


` package_alias=true`


When you set up the JFrog CLI, it overrides your native package-manager’s commands and runs the right JFrog CLI command underneath. No` jf` prefix is needed and there are no changes to your build scripts. You update the parameter once and never think about it again.


Note: When customers are using the native package tool wrapper like` mvnw` &` gradlew` in those cases` package_alias` wouldn’t work as those wrappers point to the absolute path of binaries.


**Key takeaway:** What concerned teams was never the CLI itself, rather it was the CI script changes it used to require. The JFrog CLI now deletes that step by setting one parameter when you install it in the setup process. First, it leaves your build commands as they are. Second, you benefit from faster transfers, a single sign-in, and the build data[JFrog Xray](https://jfrog.com/xray) needs to scan.


## Three examples of JFrog CLI requiring script editing


The friction between the ease-of-use and efficiency of JFrog CLI and the need for script editing should be considered from the perspective of various users:


- **You’re new to JFrog.** You’re wiring up your first pipeline and discover that “use the CLI” means rewriting commands you already know how to run.
- **You’re adopting a new package type.** For example, a project that was Maven-only starts pulling in` npm` or Go, and suddenly the CI scripts need another round of edits to keep CLI coverage intact.
- **You want to turn on security scanning.** JFrog Xray checks every package in a build for vulnerabilities and license issues, but first it needs the build data that records what’s in the build. Producing that data used to require significant edits to the CLI script.


In every case the blocker was the same: Using JFrog CLI meant changing the scripts.


**No more! From now on, JFrog CLI removes that blocker entirely.**


## Running native package commands through JFrog CLI


With` package_alias=true` configured at setup, a build step that runs` npm install` , for example, goes through the JFrog CLI, which runs the matching CLI operation and passes control back to the native tool.


A few things make it safe to roll out widely:


- **It coexists with what you already have.** Existing` jf` -prefixed commands keep working with no conflict, so you can turn on this feature without auditing legacy scripts first.
- **New package manager types need zero updates.** Adopt a new package manager later and there’s nothing to change. Just bring your native commands as is. The JFrog CLI covers it automatically, so your CI scripts stop being a maintenance item every time your stack grows.
- **You set it once at installation, not on every command.** Maintenance becomes a non-issue, with nothing added to your development workflow.


## Example: How to set up JFrog CLI for GitHub Actions


In GitHub Actions, that switch lives on the setup step:


```text


jobs:
build:
runs-on: ubuntu-latest
steps:
- uses: actions/checkout@v4
- name: Set up the JFrog CLI (Frictionless CLI on)
uses: jfrog/setup-jfrog-cli@v4
with:
package_alias: true        # one switch — no jf prefixes anywhere below
env:
JF_URL: ${{ secrets.JFROG_URL }}
JF_ACCESS_TOKEN: ${{ secrets.JFROG_ACCESS_TOKEN }}
- name: Build
run: |
npm install
npm run build


```


Notice the build step, it is a plain` npm install` , with no prefix and no JFrog-specific syntax. The pipeline looks like any other. It just does more behind the scenes.


## What is JFrog Build-Info, and how does it empower JFrog Xray?


Behind the scenes, JFrog CLI generates` Build-Info` . This is the essential data that it records including your dependencies, the machine and CI process that produced the build, and more. This crucial metadata, regarding your assets and how they were created, is JFrog’s way of defining, structuring, and capturing all of that data into one consistent record — and JFrog CLI is the tool that is responsible for producing it.


Your application security relies on this` Build-Info` as this is what JFrog Xray reads for vulnerabilities and licensing issues. So with JFrog CLI in place, turning on scanning stops being a scripting project. The data Xray needs is already being generated, with nothing added to your code. Scanning becomes something you switch on rather than a project you need to manage.


**For GitHub Actions, there’s an added benefit** as the build name and number are captured automatically from the runner, so you don’t need to input them manually into the script.


## What else can JFrog CLI do?


Build metadata is only one benefit of letting JFrog CLI handle your commands. There are several more key features and none of them require editing your scripts:


- **Parallel, chunked uploads.** Large artifacts, such as a 10 GB file, are split into chunks and transferred in parallel, which is markedly faster than pushing them through the REST API.
- **Persistent authentication.** Log in once and keep working; you don’t need to re-authenticate every time you switch repositories or integrations.
- **Broad ecosystem coverage.** JFrog CLI supports all the major repositories in the package-manager ecosystem, covering more than 20 leading package types. So whatever your teams build with, JFrog CLI has you covered.
- **Available to everyone.** JFrog CLI is included on every JFrog licensing tier and available as an[open source package on GitHub](https://github.com/jfrog/jfrog-cli) .


## How do you get started with JFrog CLI?


JFrog CLI reframes the resources required to adopt or expand the[JFrog Platform](https://jfrog.com/platform/) . New developers get to use the same familiar open source commands that they always have. Existing teams can add packages without additional scripting. And in terms of security scanning, the build metadata JFrog Xray needs to secure your software supply chain is already there.


See our[download page](https://jfrog.com/getcli/) and read the[JFrog CLI documentation](https://docs.jfrog.com/integrations/docs/jfrog-cli) to get started, or[talk to our team](https://jfrog.com/) about how to roll it out across your entire development ecosystem.
