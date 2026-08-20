---
schema_version: "1.0.0"
document_id: "b1fc23fef2c1f9f62beec5479b942d7f901cd0a9ecdb80df08b724d9ba845b12"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/now-available-bun-support-app-platform"
published_at: "2025-12-19T03:17:43.630+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T20:55:00.595584+00:00"
content_hash: "sha256:74aa102951ae87a72dbc79e4de6cbba46e9e32916bea0de8f4af58faeffa8c9f"
---

# Speed Up Your JavaScript Apps: Native Bun Support is Now Available on App Platform

The JavaScript ecosystem is rapidly evolving to meet the growing performance and integration demands of modern, AI-driven applications.[Bun](https://github.com/oven-sh/bun) is a popular framework for developers that offers an all-in-one runtime, bundler, and package manager and is considered a drop-in replacement for Node.js. With its[faster](https://bun.com/docs) startup times and lower memory usage than traditional runtimes, Bun is a top request from our customers.


Today, we’re excited to announce **native Bun support on DigitalOcean App Platform.** Now, you can deploy Bun applications directly from your code repository without writing a single line of configuration. App Platform’s Cloud Native Buildpacks will automatically detect, build, deploy and run your Bun apps.


# Benefits


-


**Performance:** Bun is a modern, high-performance JavaScript runtime with integrated tooling, optimized for fast startup and efficient execution.


-


**Zero Configuration:** You don’t need to maintain a` Dockerfile` . Just push your code into your git repo, and we’ll handle the runtime setup.


-


**Seamless Next.js Support:** Bun works natively with Next.js, allowing you to deploy your full-stack React applications with the same ease.


# Deployment Paths in App Platform


App Platform supports three distinct workflows as part of deployment.


1.


**Cloud Native Buildpacks (The “Code-First” Way):** You simply connect a GitHub, GitLab, or Bitbucket repository. We analyze your code, detect the language, and build it automatically.


2.


**Dockerfiles:** You include a` Dockerfile` in your repo. We detect it and build the image according to your instructions.


3.


**Pre-Built Images:** You build the container image locally and push it to the DigitalOcean Container Registry (DOCR). We deploy exactly what you pushed.


# How to Get Started with Bun


If you’re new to Bun, getting started is straightforward. Bun is designed to be a drop-in replacement for Node.js, meaning you can often use your existing code and package.json with minimal changes.


- **Install Bun:** Fast-track your setup by installing Bun globally on your local machine. Bash


None


```text


# Install via curl


curl -fsSL https://bun.sh/install | bash


```


- **Run Your App:** Use bun install (just like you would npm install for[node.js](http://node.js/) ) for fast dependency management, and bun run to execute your scripts. Bash


None


```text


# Install dependencies


bun install


# Run your dev script


bun run dev


```


- **Learn More:** For a deep dive into Bun’s runtime APIs and advanced usage, check out[Bun Documentation](https://bun.com/docs) .


# How Bun Detection Works


Once you’ve initialized your project locally and made sure it is working, App Platform identifies it as a Bun application. Our build system looks at your repository’s root directory to determine the correct runtime.


-


` Detection: App Platform confirms if you are using Bun if it detects this specific lockerfile in your repository:`


` bun lock`


Once detected, the buildpack takes over:


-


**Install:** Runs bun install to fetch dependencies. (Note: If your bun.lock hasn’t changed, App Platform re-uses the cached node_modules to speed up the build).


-


**Build:** Runs bun run build. You can override this by defining a build_command in the App Spec. To run actions before or after dependencies install, you can also use digitalocean-prebuild and digitalocean-postbuild package.json scripts.


-


**Start:** Defaults to` bun run start` to launch your application.


The bun version is selected in the following order:


1.


From the BUN_VERSION environment variable.


2.


From .bun-version file.


3.


From .runtime.bun.txt file.


4.


If no version is specified, it uses the latest version from Bun’s GitHub Releases. App Platform will cache your Bun and Node versions to ensure faster subsequent builds unless you change the requested version.


# Migrating from Node.js to Bun


If you choose to convert an existing Node.js application on App Platform to use the Bun runtime, the process is simple. You don’t need to change application code, just project metadata.


-


Step 1: Remove Node Artifacts. Delete your package-lock.json (or yarn.lock). This tells App Platform “stop treating this as a standard Node app.”


-


Step 2: Generate Bun Artifacts. Run bun install locally. This generates the bun.lock file.


-


Step 3: Push Changes. Commit these changes to your repository. When App Platform sees the new commit, it will drop the Node buildpack and pick up the Bun buildpack automatically.


# Use Bun as Package Manager


If you intend to use Bun as your package manager but Node.js as your runtime, Node.js will be installed in the following scenarios:


- If you define bun as Package Manager in your` package.json` . Update your` package.json` as follows:


` JSON`


None


```text


{


"packageManager": "bun@1.2.0"


}


```


- If any script in` package.json` contains node, like this:


` JSON`


None


```text


{


"scripts":  {


"start": "node src/index.js"


}


}


```


- If Bun is used as package-manager, it will install and use **Node.js v22.x** by default. If you want to install a specific Node.js version, you can define it in the` engines` section of` package.json` , as follows:


` JSON`


None


```text


{


"engines":  {


"node": "23.x"


}


}


```


# Next.js Configuration


You can deploy[Next.js](http://next.js/) applications as well on App Platform with bun runtime. If you are deploying a **Next.js** project (including those using[ISR](https://nextjs.org/docs/app/guides/incremental-static-regeneration) ), you must update your scripts to explicitly use the Bun runtime for the build and dev processes.


Update your` package.json` as follows:


` JSON`


None


```text


{


"scripts":  {


"dev": "bun --bun next dev",  //or remove this line, as app platform uses the build script


"build": "bun --bun next build"


}


}


```


This ensures that Next.js utilizes Bun’s runtime features correctly during the build process on App Platform.


# Get Started


Native Bun support in DigitalOcean App Platform is available in all regions today.


-


**New App?** Just link your GitHub repo containing a` bun lock` file.


-


**Existing App?** Switch your package manager and push the update to trigger a new build.


Check out the[official Bun Buildpack documentation](https://docs.digitalocean.com/products/app-platform/reference/buildpacks/bun/) for advanced configuration options.
