---
schema_version: "1.0.0"
document_id: "b624091b49dba51384dabae09334fb9a79f0f784e0c4f82f3472ab679e9523f3"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-12b6d71fe86e"
canonical_url: "https://www.windmill.dev/blog/windows-workflow-engine"
published_at: "2026-04-29T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:07.070377+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:815389d63be3005684db0c70680a014d9e6165836f9fd268d86390d6431b88ac"
---

# Native Windows automation without Docker, WSL, or workarounds

You manage Windows servers, Active Directory, MSSQL, and PowerShell runbooks. You need to automate server provisioning, user management, database maintenance, and compliance checks. You also need version control, approval workflows, and audit trails around all of it.


The usual answer is "install Docker" or "set up WSL2." But your PowerShell scripts need access to AD, your MSSQL queries use Kerberos tickets from the domain, and your modules come from private Azure Artifacts feeds. Containerizing all of that means fighting your own infrastructure.


Windmill runs as a native Windows service.` windmill-ee.exe` registers via` sc.exe` and executes PowerShell, C#, and SQL directly on your domain-joined servers. This post walks through what that enables: typed PowerShell with` \[CmdletBinding()\]` , MSSQL with Kerberos auth, Teams approvals, and AI agents that use all of the above as tools.


## Native Windows workers​


Windmill's Windows worker is a single executable:` windmill-ee.exe` . It runs anywhere Windows runs: servers, workstations, laptops, edge devices. Three modes:


- **Worker** : connects directly to the Windmill database, executes jobs
- **Server** : runs the Windmill API and web UI
- **Agent** : connects to a remote Windmill server over HTTPS, with no direct database access. Ideal for workers behind firewalls. See[agent workers](https://www.windmill.dev/docs/core_concepts/agent_workers) for how to generate tokens


### Installing as a Windows service​


```text
sc  .  exe create WindmillWorker `      binPath=   "C:\windmill\windmill-ee.exe"   `        start  = auto `      DisplayName=   "Windmill Worker"         $regPath   =   "HKLM:\SYSTEM\CurrentControlSet\Services\WindmillWorker"      $envVars   = @  (           "MODE=worker"  ,           "DATABASE_URL=postgres://postgres: [email protected]  :5432/windmill?sslmode=disable"  ,           "WORKER_TAGS=powershell,csharp,mssql"  ,           "SKIP_MIGRATION=true"      )      Set-ItemProperty     -  Path   $regPath     -  Name   "Environment"     -  Value   $envVars     -  Type   MultiString
```


Agent mode (connecting to a remote server without database access):


```text
sc  .  exe create WindmillAgent `      binPath=   "C:\windmill\windmill-ee.exe"   `        start  = auto `      DisplayName=   "Windmill Agent"         $regPath   =   "HKLM:\SYSTEM\CurrentControlSet\Services\WindmillAgent"      $envVars   = @  (           "MODE=agent"  ,           "BASE_INTERNAL_URL=http://your-windmill-server:8000"  ,           "AGENT_TOKEN=jwt_agent_your_token_here"      )      Set-ItemProperty     -  Path   $regPath     -  Name   "Environment"     -  Value   $envVars     -  Type   MultiString
```


Configure automatic restart on failure:


```text
sc  .  exe failure WindmillWorker reset= 86400 actions= restart/60000/restart/60000/restart/60000     sc  .  exe   start   WindmillWorker
```


[NSSM](https://nssm.cc/) is also supported as an alternative service manager.


### Mixed Windows/Linux fleet​


A single Windmill instance can run both Linux and Windows workers. Use[worker tags](https://www.windmill.dev/docs/core_concepts/worker_groups) to route jobs: tag your Windows workers with` powershell` ,` csharp` , or` windows` , and flows dispatch steps to the right OS automatically.


[Windows workers Set up Windmill workers on Windows as native services.](https://www.windmill.dev/docs/misc/windows_workers)[Workers and worker groups Assign scripts to specific worker groups by tags.](https://www.windmill.dev/docs/core_concepts/worker_groups)


## PowerShell as a first-class language​


Windmill parses` param()` blocks to infer typed parameters and generates a UI form automatically. PowerShell scripts get the same capabilities as Python or TypeScript: schedules, webhooks, approval flows, error handling, and composition in[flows](https://www.windmill.dev/docs/flows/flow_editor) .


### Querying Active Directory​


```text
param  (           [string]  $GroupName  ,           [string]  $Domain   =   "corp.contoso.com"      )         Import-Module   ActiveDirectory        $members   =   Get-ADGroupMember     -  Identity   $GroupName     -  Server   $Domain     |           Select-Object   Name  ,   SamAccountName  ,   ObjectClass   |           ForEach-Object     {               [PSCustomObject]  @  {                 name =   $_  .  Name                username =   $_  .  SamAccountName                  type   =   $_  .  ObjectClass              }           }         $members     |     ConvertTo-Json
```


` $GroupName` becomes a required text input,` $Domain` gets a default value, and Windmill auto-generates the UI form. The JSON output is directly consumable by downstream flow steps.


### CmdletBinding and common parameters​


When a script declares` \[CmdletBinding()\]` , Windmill shows toggles for` -Verbose` ,` -Debug` , and an` -ErrorAction` dropdown in the run form. These are the same common parameters you'd use in a PowerShell console.` Write-Verbose` and` Write-Debug` output appears in job logs with` VERBOSE:` /` DEBUG:` prefixes. Scripts without` \[CmdletBinding()\]` are unaffected.


```text
[CmdletBinding()]      param  (           [Parameter(Mandatory=$true)]           [string]  $ServerName  ,              [  ValidateSet  (  "Full"  ,     "Quick"  ,     "Security"  )  ]           [string]  $ScanType   =   "Quick"      )         Write-Verbose     "Starting   $ScanType   scan on   $ServerName  "      $result   =   Invoke-ServerScan     -  Server   $ServerName     -  Type     $ScanType      Write-Debug     "Raw result:   $  (  $result     |     ConvertTo-Json     -  Depth 1  )  "      $result     |     ConvertTo-Json
```


### Private Azure Artifacts modules​


Configure your Azure Artifacts feed URL and PAT in[instance settings](https://www.windmill.dev/docs/advanced/imports#private-repository-azure-artifacts-feed) , then use` Import-Module` as usual. Windmill checks the private feed first and falls back to PowerShell Gallery.


```text
param  (           [string]  $ServerName  ,           [string[]]  $Checks   = @  (  "cpu"  ,     "memory"  ,     "disk"  )      )         Import-Module   CompanyInternalTools        $results   =   Invoke-ServerHealthCheck     -  ServerName   $ServerName     -  Checks   $Checks      $results     |     ConvertTo-Json
```


[PowerShell quickstart Write and run PowerShell scripts with typed parameters.](https://www.windmill.dev/docs/getting_started/scripts_quickstart/bash)


## MSSQL with Kerberos​


Windmill's[MSSQL integration](https://www.windmill.dev/docs/integrations/mssql) supports three auth methods: SQL auth (username/password), Azure AD/Entra (OAuth), and Windows Integrated Authentication (Kerberos).


With Kerberos, the worker's service account credentials are used directly, with no database passwords stored in Windmill. The worker inherits its domain identity, and MSSQL trusts it through AD.


Requirements:


- Valid Kerberos ticket on the worker (via` kinit` or a keytab)
- Correct` krb5.conf` realm configuration
- Service account permissions on the target database


### Resource config​


```text
{         "host"  :     "sql01.corp.contoso.com"  ,         "dbname"  :     "AppDatabase"  ,         "integrated_auth"  :     true  ,         "encrypt"  :     true  ,         "trust_cert"  :     false      }
```


No` user` or` password` fields. Then query with a[SQL script](https://www.windmill.dev/docs/getting_started/scripts_quickstart/sql) :


```text
-- database u/admins/mssql_kerberos      SELECT         e  .  EmployeeID  ,         e  .  DisplayName  ,         e  .  Department  ,         e  .  LastLogin     FROM   HR  .  Employees e     WHERE   e  .  Department   =   $  1         AND   e  .  IsActive   =     1      ORDER     BY   e  .  LastLogin   DESC
```


The` -- database` directive pins the script to the Kerberos-authenticated resource.


[MS SQL integration Connect to MSSQL with SQL auth, Azure AD, or Kerberos.](https://www.windmill.dev/docs/integrations/mssql)[SQL quickstart Run SQL queries against PostgreSQL, MySQL, MSSQL, BigQuery, and Snowflake.](https://www.windmill.dev/docs/getting_started/scripts_quickstart/sql)


## C#/.NET 9​


C# scripts run natively with .NET 9. NuGet dependencies are declared inline and resolved automatically.


```text
//requirements:    // [email protected]        using Azure.Storage.Blobs;    using Azure.Storage.Blobs.Models;       public class Main    {        public static async Task<object> main(            string connectionString,            string containerName,            string blobName)        {            var client = new BlobServiceClient(connectionString);            var container = client.GetBlobContainerClient(containerName);            var blob = container.GetBlobClient(blobName);               BlobDownloadResult result = await blob.DownloadContentAsync();            string content = result.Content.ToString();               return new {                blob_name = blobName,                size_bytes = result.Details.ContentLength,                content_type = result.Details.ContentType,                last_modified = result.Details.LastModified            };        }    }
```


On a Windows worker this runs on the installed .NET runtime with full access to Windows APIs and the local filesystem.


[C# quickstart Write and run C# scripts with NuGet dependency management.](https://www.windmill.dev/docs/getting_started/scripts_quickstart/csharp)


## Microsoft ecosystem integrations​


### Teams​


Windmill's[Teams integration](https://www.windmill.dev/docs/integrations/teams) covers three patterns:


**Commands** :` /windmill` triggers in Teams channels execute scripts on any worker, including Windows workers, and reply with results.


**Approval steps** : flows pause and request approval in a Teams channel. Approvers can approve, reject, or modify parameters inline.


```text
await   wmill  .  requestInteractiveTeamsApproval  (  {         teamName  :     "Infrastructure"  ,         channelName  :     "Approvals"  ,         message  :     "Server decommission request: SRV-DB-03"  ,         approver  :     "ops-lead"  ,         defaultArgsJson  :     {   server  :     "SRV-DB-03"  ,   action  :     "decommission"     }  ,         dynamicEnumsJson  :     {   action  :     [  "decommission"  ,     "reboot"  ,     "snapshot"  ]     }  ,      }  )  ;
```


**Error alerts** : flow failures send notifications to Teams with full error context.


### Azure AD/Entra ID SSO​


Windmill supports SSO via[Azure AD OAuth](https://www.windmill.dev/docs/advanced/setup_oauth) . The same identity can authenticate against MSSQL using` aad_token` , so one login covers both platform access and database access.


### Azure Blob Storage​


Native[Azure Blob integration](https://www.windmill.dev/docs/integrations/microsoft-azure-blob) for file operations in flows.


### Kafka with GSSAPI/Kerberos​


Enterprise Kafka clusters in AD environments often use Kerberos. Windmill's[Kafka triggers](https://www.windmill.dev/docs/triggers/kafka_triggers) support` SASL_GSSAPI` and` SASL_SSL_GSSAPI` with keytab support (file path or base64-encoded):


```text
kerberos_service_name  :   kafka     kerberos_principal  :   windmill  -  [email protected]      keytab_path  :   /etc/security/keytabs/windmill.keytab
```


[Teams integration Trigger scripts from Teams, request approvals, and receive notifications.](https://www.windmill.dev/docs/integrations/teams)[Kafka triggers Trigger flows from Kafka messages with GSSAPI/Kerberos support.](https://www.windmill.dev/docs/triggers/kafka_triggers)


## AI agents on Windows infrastructure​


Running PowerShell against a domain-joined server or querying MSSQL with Kerberos auth isn't something most AI agent platforms can do.


Windmill's[AI agent](https://www.windmill.dev/docs/core_concepts/ai_agents) flow steps use scripts as tools. Any Windmill script (PowerShell, C#, SQL, TypeScript) becomes a callable tool where typed parameters map to the tool's JSON schema. The agent sees what each tool does and what inputs it needs, then calls them to accomplish a goal.


### Scenario: AI IT ops assistant​


An AI agent in a Windmill flow with these tools:


- A PowerShell script that queries AD for user/group information
- A PowerShell script that checks Windows Event Logs
- An MSSQL query that pulls application error data
- A Teams notification script for escalation


The agent receives a natural-language request like "check if user jsmith's account is locked and what errors their app generated today". It reasons about which tools to call, executes them on the Windows worker, and returns a structured report or escalates via Teams.


Each tool is a regular Windmill script. Here's the AD lookup tool. The agent calls it by name and fills in the` $Username` parameter:


```text
[CmdletBinding()]      param  (           [Parameter(Mandatory=$true)]           [string]  $Username      )         Import-Module   ActiveDirectory        $user   =   Get-ADUser     -  Identity   $Username     -  Properties LockedOut  ,   LastLogonDate  ,   MemberOf  ,   Enabled     Write-Verbose     "Querying AD for user:   $Username  "         [PSCustomObject]  @  {         username    =   $user  .  SamAccountName        locked_out  =   $user  .  LockedOut        enabled     =   $user  .  Enabled        last_logon  =   $user  .  LastLogonDate        groups      =   (  $user  .  MemberOf   |     ForEach-Object     {     (  $_     -  split   ','  )  [  0  ]     -replace     'CN='     }  )      }     |     ConvertTo-Json
```


In the flow editor, the agent step wires these tools together with a system prompt and iterates over affected users:


The agent calls` get_ad_user_info` , sees the account is locked, calls` get_event_log_errors` to check for related authentication failures, runs` query_app_errors` for today's errors, and compiles a report. If it finds critical issues, it calls` send_teams_alert` to escalate. Each tool runs on the appropriate worker: PowerShell and MSSQL on the Windows worker with domain credentials, TypeScript on any available worker.


### Multi-provider and MCP​


Windmill's agents work with OpenAI, Anthropic, Azure OpenAI (for Microsoft shops needing data residency), Mistral, and others. Windmill also exposes itself as an[MCP server](https://www.windmill.dev/docs/core_concepts/mcp) , so external AI tools (Claude, Cursor) can trigger scripts on your Windows infrastructure:


```text
claude mcp   add     --transport   http windmill https://windmill.corp.contoso.com/api/mcp/w/prod/mcp?token  =  your_token
```


[AI agents Build AI agent steps in flows with scripts as tools.](https://www.windmill.dev/docs/core_concepts/ai_agents)[MCP integration Expose Windmill scripts as tools for external AI clients.](https://www.windmill.dev/docs/core_concepts/mcp)


## Replacing Task Scheduler and Power Automate​


The typical migration path: PowerShell scripts scattered across servers on Task Scheduler, Power Automate for approvals, alerts via email, no audit trail.


In Windmill, each script becomes version-controlled, gets an auto-generated UI, and can be composed into flows with approval steps (via[Teams](https://www.windmill.dev/docs/integrations/teams) ), error handling, and retries. Windows and Linux workers run in the same instance. Every execution is logged with inputs, outputs, duration, and who triggered it.


The migration is incremental: start with one runbook, schedule it in Windmill, and let the Task Scheduler job run in parallel until you're confident.


## How Windmill compares on Windows​


[Airflow](https://airflow.apache.org/) has had an[open issue for Windows support](https://github.com/apache/airflow/issues/10388) since 2020, and it's not on the roadmap.[Prefect](https://www.prefect.io/) workers can run on Windows via pip, but the docs focus on Docker/Kubernetes and there's no service management.[n8n](https://n8n.io/) runs well via Node.js but is low-code and connector-focused.[Kestra](https://kestra.io/) is the closest: it runs as a JAR, has a PowerShell plugin, and supports a PROCESS runner, but lacks typed parameter inference, private Azure Artifacts, MSSQL Kerberos, and C# support.


Power Automate has strong Teams integration, but no self-hosted option, no version control, no code-first workflow definition, and premium licensing at $15/user/month. That means no GitOps, no PR reviews on flow changes, and no self-hosted deployment.


Capability Windmill Kestra n8n Airflow Prefect Power Automate


Runs on Windows Native service JAR file Node.js No (WSL only) Partially Cloud only


Windows service mgmt sc.exe/NSSM No No No No N/A


PowerShell typed params Yes + CmdletBinding Basic plugin No No No Actions only


C#/.NET scripts Yes (.NET 9) No No No No No


MSSQL Kerberos auth Yes No No No No Via connector


Kafka GSSAPI/Kerberos Yes Plugin No No No Via connector


Code-first Yes Yes Low-code Yes Yes No


Version control Git-native Git-native Git Git-native Git-native No


Self-hosted Yes Yes Yes Yes Yes No


## Getting started​


The Windmill server runs in Docker (Linux). Windows workers connect as native agents, with no Docker on the Windows side.


1. Deploy the[Windmill server](https://www.windmill.dev/docs/advanced/self_host) (Docker Compose or Kubernetes)
2. Download` windmill-ee.exe` on your Windows server
3. Register it as a Windows service in agent mode
4. Connect with an agent token


Windows workers require an[Enterprise license](https://www.windmill.dev/pricing) .


[Windows workers setup Install and configure Windmill workers on Windows.](https://www.windmill.dev/docs/misc/windows_workers)[Enterprise pricing Windows workers, SSO, audit logs, and more.](https://www.windmill.dev/pricing)


[Windmill](https://www.windmill.dev/) is an[open-source](https://github.com/windmill-labs/windmill) and[self-hostable](https://www.windmill.dev/docs/advanced/self_host/) developer platform to build, orchestrate, and monitor internal tools and data pipelines, combining the power of code with the velocity of low-code. We turn your scripts into internal apps and composable steps of flows that automate repetitive workflows.


You can[self-host](https://www.windmill.dev/docs/advanced/self_host/) Windmill using a` docker compose up` , or go with the[cloud app](https://app.windmill.dev/user/login) .
