---
schema_version: "1.0.0"
document_id: "43ba1b2a1bab06e30bf540c20461741a334e2c63d2f2abf7d1cce62c2b6cab01"
company_key: "nutanix-inc-class-a-common-stock"
company: "Nutanix Inc."
source_id: "nutanix-inc-class-a-common-stock-rss-12a2d78c04c7"
canonical_url: "https://www.nutanix.dev/2026/06/30/announcing-the-nutanix-sdk-for-microsoft-c/"
published_at: "2026-06-30T14:00:00+00:00"
first_seen_at: "2026-07-20T03:31:13.386524+00:00"
fetched_at: "2026-08-20T00:46:45.571645+00:00"
content_hash: "sha256:645376c4e07c4ff9ea3b6933288d2b69f0216dca206ea9466a13f5124436d710"
---

# Announcing the Nutanix SDK for Microsoft C#

In May 2026 Nutanix expanded our v4 API capabilities with the Nutanix SDK for Microsoft C#. The addition of a C# language-specific SDK expands the list of supported languages as follows.


- Microsoft C#
- Python
- Java
- JavaScript
- Go


The C# SDK provides support for cross-platform development using Microsoft C# 10.0 on Windows, Mac OS X and Linux.


## Assumptions


For consistency, this demo will use command-line tools for cloning the repository, creating a .NET project as well as the final build and run steps.


These steps can be completed your choice of IDE e.g. VS Code, Visual Studio or any other compatible tool of your choice.


## Environment Setup


.NET development requires installation of the .NET SDK. The examples below are not an exhaustive list of all installation methods and should be used for examples purposes only.


At the time of publication, the .NET SDK for all supported operating systems can be downloaded from the[Microsoft Download .NET](https://dotnet.microsoft.com/en-us/download) page.


### Windows


After downloading the appropriate installer, run the executable and follow on-screen instructions.


### Mac OS X


#### Option 1: Direct Link


Current latest .NET SDK download at the time of publication:[https://builds.dotnet.microsoft.com/dotnet/Sdk/10.0.301/dotnet-sdk-10.0.301-osx-arm64.pkg](https://builds.dotnet.microsoft.com/dotnet/Sdk/10.0.301/dotnet-sdk-10.0.301-osx-arm64.pkg)


#### Option 2: Homebrew


If you prefer to use the Homebrew package manager, the following command can be used to install the .NET SDK:


```text
brew install --cask dotnet-sdk
```


### Linux


Due to the large number of available Linux distributions, this article cannot cover every available package management or download option. The examples below are for Rocky Linux 10.0 and Ubuntu 26.04 (LTS), although other distributions have similar methods for their own package managers.


#### Rocky Linux 10


Update and upgrade packages, then install the .NET SDK.


```text
sudo dnf update
sudo dnf upgrade
# reboot, if required
sudo dnf install dotnet-sdk-10.0
```


Installing the .NET SDK on Rocky Linux 10


#### Ubuntu 26.04


Update and upgrade packages, then install the .NET SDK.


```text
sudo apt update
sudo apt upgrade
# optional, depending on the user's environment
sudo apt dist-upgrade
# reboot, if required
sudo apt install dotnet10
```


## Project Setup


For this article we’ll offer two options:


1. An entirely new Console App project, inside which you can create any application you need.
2. Clone our NutanixDev` code-samples` GitHub repository and use the provided demo.


- This demo assumes you have a connection to and credentials for an existing Prism Central instance.
- **It is recommended new users start with Option 2.**


**Both** options use the Nutanix SDK for C#’s Virtual Machine Management (` vmm` SDK). This is for demo purposes only; other Nutanix v4 API namespaces can be used the same way.


### Option 1: New Project


1. Create a directory for your C# projects. This example uses` ~/projects/csharp` .
2. Ensure the` dotnet` command is available.


Confirm the` dotnet` command is available


1. Change to your projects directory.


```text
# cd <project-directory> e.g.
cd ~/projects/csharp
```


**Note** : For the remainder of this article,` dotnet` commands are identical for all operating systems. The example screenshots here show a Rocky Linux 10.0 system


1. Create a new .NET Console App.


```text
# dotnet new console -o <project-name> e.g.
dotnet new console -o csharp_sdk
```


Using the` dotnet` command to create a new .NET SDK Console App project


1. Navigate to the new project directory and install the Nutanix SDK for C#, specifically Virtual Machine Management (` vmm` namespace)


```text
# assumes the project is in the "csharp_sdk" subdirectory
cd csharp_sdk
# install the Nutanix SDK for C#, specifically the VMM namespace
dotnet add package Nutanix.VmmSDK --version 1.0.0
```


Install the Nutanix SDK for C#, specifically for Virtual Machine Management (` vmm` namespace)


1. This will create a new project containing the basic framework for a Nutanix SDK for C# Console App. For new users,` Project.cs` is the recommended starting place. **Note** : Option 1 creates an empty project that can be modified as you see fit.


### Option 2: Clone Demo Repository


1. Navigate to your projects directory.


```text
# cd <project-directory> e.g.
cd ~/projects/csharp
```


1. Clone the Nutanix` code-samples` repository. This will download all code samples for C# as well as our other SDKs and other REST API examples.


```text
git clone https://github.com/nutanixdev/code-samples
```


1. Navigate to the demo project directory and verify the directory contents. All required supporting files for the demo are in the top-level directory.


```text
cd code-samples/csharp/v4api_sdk/csharp_demo/
```


Verifying contents Nutanix SDK for C# Console App demo directory


1. If not already done on your system, install the Nutanix SDK for C#, specifically Virtual Machine Management (` vmm` namespace).


```text
dotnet add package Nutanix.VmmSDK --version 1.0.0
```


1. Edit` environment.json` to match your environment:


- ` pc_fqdn` : Prism Central FQDN or IP address.
- ` pc_username` : Prism Central username. To help ensure the demo app doesn’t use any hard-coded password, it will prompt for the user’s password at runtime.


1. Verify the Nutanix SDK for C# has been installed correctly and that the Console App is ready to build & run.


```text
dotnet build
```


Running` dotnet build` to verify all dependencies are met


1. Finally, run the demo Console App.


```text
dotnet run
```


As can be seen in the following screenshot, the Console App’s C# compiled binary connects to Prism Central using Nutanix v4 APIs and collects a list of all VMs found in that Prism Central instance. There are 19 VMs shown here.


As indicated earlier in this article, the user’s password is not hard-coded into the` environment.json` file and is collected at runtime.


**Note** : In an automated environment authentication would likely be managed via script-specific Users of type Service Account with an associated API key. For a complete demo of this approach using the Nutanix Python SDK, see[Nutanix v4 APIs: Using API Key Authentication (Python SDK)](https://www.nutanix.dev/2025/02/05/nutanix-v4-apis-using-api-key-authentication/) .


Running the Nutanix SDK for C# demo Console App


## Demo App Source


If you chose to follow the demo app above (Option 2), we can now look at some notes for the app’s source. Within the cloned application, the important sections are contained within` EnvConfig.cs` ,` Program.cs` and` csharp_demo.csproj` .


### csharp_demo.csproj


**Note** : The steps in this section apply to` csharp_demo.csproj` only.


Within the app’s project configuration, a single block will prevent build errors related to` environment.json` . Without this block, the .NET compiler will look for an **existing** file named` environment.json` in the build output directory. This block **copies**` environment.json` into the build output directory, preventing this error.


```text
<ItemGroup>
<None Include="environment.json" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```


### EnvConfig.cs


**Note** : The steps in this section apply to` EnvConfig.cs` only.


To layout the framework for a well-designed application, our demo uses a C#` record` to store environment-specific configuration. In this demo, environment-specific configuration includes 2 settings, both of which are stored in` environment.json` .


1. ` pc_fqdn` : Prism Central FQDN or IP
2. ` pc_username` : Prism Central username


The main` Program.cs` file will use the` EnvConfig` record during deserialization of the` environment.json` file contents.


The following screenshot demonstrates that the` environment.json` file has been copied to the directory containing` csharp_demo` i.e. the compiled binary/executable.


` environment.json` copied to build output directory


### Program.cs


**Note** : The steps in this section apply to` Program.cs` only.


#### Required` using` statements


For this demo, a small number of built-in C# libraries are used, as as well a number of models provided by the Nutanix SDK for C#.


```text
using Nutanix.VmmSDK.Api;
using Nutanix.VmmSDK.Client;
using Nutanix.VmmSDK.Model.Vmm.V4.Ahv.Config;
using Nutanix.VmmSDK.Model.Request.Vm;
using System.Text.Json;
using System.Security;
```


#### Connection and display defaults


Until the Nutanix Central 2.0 was released as GA in early 2026, all API requests were sent over HTTPs port 9440. However, various` multidomain` API and SDK functions use HTTPs port 443.


Additionally, some consoles may not support a terminal width of 120 characters. For display purposes this can be changed here.


```text
// default Prism Central port
const int DefaultPort = 9440;


// console table width
// can be modified here for consoles/terminal with different sizes
const int ConsoleTableWidth = 120;
```


#### Read JSON from` environment.json`


` AppContext.BaseDirectory` refers to the build output path, i.e. where the` environment.json` file is copied before debugging.


```text
// load the environment configuration from environment.json
var jsonPath = Path.Combine(AppContext.BaseDirectory, "environment.json");
var jsonContent = File.ReadAllText(jsonPath);
var jsonConfig = JsonSerializer.Deserialize<EnvConfig>(jsonContent) ?? throw new InvalidOperationException("Failed to load environment configuration");
```


#### Read password from console


The` ReadPassword` function collects a password from the user, stored as a` SecureString` . For our demo, this helps ensure no passwords are hard-coded into` environment.json` .


```text
/// <summary>Reads a password from console input without echoing characters to the screen.</summary>
SecureString ReadPassword(string prompt = "Password: ")
{
...
return password;
}


// get the user's password
using var password = ReadPassword("Enter password:");
```


#### Prism Central connection


The remaining steps are very similar to those carried out when using any other Nutanix SDK:


- Instantiate the` Configuration` instance; Prism Central IP address (etc).
- The API client object that makes use of the` Configuration` instance.
- The API instance that specifies which` vmm` API will be used in this demo.


```text
// setup the connection configuration
var config = new Configuration()
{
Host = jsonConfig.pc_fqdn,
Port = DefaultPort,
Username = jsonConfig.pc_username,
Password = password,
VerifySsl = false
};


// setup the API client and VPI instance
var client = new ApiClient(config);
var vmApi = new VmApi(client);
```


#### Generate VM List


Finally, our app preparation allows the connection to Prism Central, followed by a request to list all virtual machines. This section makes use of the` ConsoleTableWidth` constant; this constant can be adjusted to fit your terminal in the event the output is not readable.


With the VM list returned, the demo iterates over the list, grabbing the VM name, power state and` extId` before displaying them in a formatted table.


In the event an SDK error occurs during this process, the Nutanix SDK for C#’s custom` ApiException` is caught, with exception details shown.


```text
// try to list the VMs, following up with a list of those VMs
// display an appropriate message if the VM list request fails
try
{
var request = new ListVmsRequest();
var response = vmApi.ListVms(request);
var vms = response.Data as List<Vm>;


if (vms == null || vms.Count == 0)
{
Console.WriteLine("No virtual machines found.");
return;
}


Console.WriteLine($"{"Name",-70} {"Power State",-15} {"UUID"}");
Console.WriteLine(new string('-', ConsoleTableWidth));


foreach (Vm vm in vms)
{
Console.WriteLine($"{vm.Name,-70} {vm.PowerState.ToString() ?? "unknown",-15} {vm.ExtId}");
}


Console.WriteLine($"\nTotal: {vms.Count} VM(s)");
}
catch (ApiException ex)
{
Console.Error.WriteLine($"API error {ex.ErrorCode}: {ex.Message}");
}
```


## Conclusion


This demo has demonstrated how the Nutanix SDK for C# can be used to connect to Prism Central and use the Virtual Machine Management namespace (` vmm` ) to generate a formatted list of virtual machines. This is for demo purposes only; other Nutanix v4 API namespaces can be used the same way.


Of course, this example is very high-level and just the starting point for what the Nutanix SDK for C# can accomplish, but should serve as good a starting point for using our new C# SDK.


## Related Resources


- [VmmSDK on NuGet](https://www.nuget.org/packages/Nutanix.VmmSDK)
- [VmmSDK.API documentation](https://developers.nutanix.com/sdk-reference?namespace=vmm&version=v4.2&language=csharp) on the Nutanix v4 Developer Portal
