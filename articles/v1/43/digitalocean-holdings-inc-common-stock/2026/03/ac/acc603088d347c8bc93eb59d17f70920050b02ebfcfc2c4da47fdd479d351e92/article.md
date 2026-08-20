---
schema_version: "1.0.0"
document_id: "acc603088d347c8bc93eb59d17f70920050b02ebfcfc2c4da47fdd479d351e92"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/net-buildpack-support-app-platform"
published_at: "2026-03-05T21:21:25.719+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:54846df86541cf3302f72d99e4374e3dfaa7a5c5f3806a6125ac467e290c1a04"
---

# Native .NET Buildpack Support is Now Available on App Platform

The .NET ecosystem continues to power a significant share of enterprise and cloud-native applications, from web APIs and microservices to full-stack applications built with[ASP.NET](http://asp.net/) Core. Developers building with C#, F#, and Visual Basic need a deployment experience that matches the productivity of the framework itself: push code, and let the platform handle the rest.


Today, we’re excited to announce **native .NET buildpack support on DigitalOcean App Platform.** You can now deploy your .NET applications directly from a Git repository without writing or maintaining Dockerfiles. App Platform automatically detects your .NET project, installs the correct SDK version, and builds your application for production.


### Benefits


- **Zero Configuration:** Push your .NET code to a Git repository, and App Platform handles runtime detection, SDK installation, and build configuration automatically—no Dockerfile required.
- **Multi-Language Support:** Build applications in C#, Visual Basic, or F# using the .NET and[ASP.NET](http://asp.net/) Core frameworks, all with the same streamlined deployment experience.
- **Automatic SDK Management:** App Platform selects the appropriate .NET SDK version based on your project’s TargetFramework or global.json configuration, supporting .NET 8.0, 9.0, and 10.0.
- **Production-Ready Defaults:** The buildpack compiles with the Release configuration by default and automatically detects[ASP.NET](http://asp.net/) Core web applications to configure the correct process type.


### How Detection Works


Once you connect your Git repository, App Platform identifies your application as a .NET project by looking for specific files in your repository root.


App Platform confirms a .NET application if it detects any of the following:


-


Solution files: *.sln, *.slnx


-


Project files: *.csproj, *.vbproj, *.fsproj


-


File-based apps: *.cs


Once detected, the buildpack takes over:


- **SDK Detection:** Determines the required .NET SDK version from your TargetFramework property or global.json file
- **Dependency Restore:** Runs dotnet restore to fetch NuGet packages
- **Build & Publish:** Runs dotnet publish with the Release configuration
- **Process Registration:** Automatically registers[ASP.NET](http://asp.net/) Core projects as web process types


### Supported Runtimes


App Platform uses the Heroku .NET buildpack (version 42) and supports the following SDK versions on Ubuntu 22:


The buildpack supports Target Framework Moniker (TFM) values in the format net{major_version}.0, such as net8.0, net9.0, and net10.0.


### How to Get Started


Deploying a .NET application to App Platform takes just a few steps:


- **Via Control Panel:** Create a new app, connect your Git repository, and App Platform automatically detects your .NET project and configures the build.
- **Via CLI:** Use doctl apps create with an app spec pointing to your repository.
- **Via API:** Call the Apps API to create and deploy your application programmatically.


For applications that need to bind to a specific port, ensure your code reads from the PORT environment variable:


CSharp


```text


var   port = Environment.GetEnvironmentVariable( "PORT"  ) ??  "5000"   ;


builder.WebHost.UseUrls($"http://*:{port}")   ;


```


Check out the[.NET Buildpack documentation](https://docs.digitalocean.com/products/app-platform/reference/buildpacks/dotnet/) for advanced configuration options including custom SDK versions, build configurations, and MSBuild verbosity settings.


Native .NET buildpack support is available in all App Platform regions today. Connect your repository and deploy your first .NET application in minutes.


Check out the[official documentation](https://docs.digitalocean.com/products/app-platform/reference/buildpacks/dotnet/) to get started.
