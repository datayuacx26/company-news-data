---
schema_version: "1.0.0"
document_id: "cc546e0f9838388632c82a68671fccac9e39acfbfffd9574041a6b161f95c1fa"
company_key: "yc-brownie"
company: "IncidentFox"
source_id: "yc-brownie-news-import-2b90dab87e5f"
canonical_url: "https://www.incidentfox.ai/blog/automated-incident-triage-with-ai.html"
published_at: null
first_seen_at: "2026-07-23T11:48:55.002823+00:00"
fetched_at: "2026-07-28T21:20:09.527818+00:00"
content_hash: "sha256:372e21150d6d9c39640a8fe35d2905a3886f4167901ed140924a3e3d33f68c13"
---

# How to Set Up Automated Incident Triage with AI

When an alert fires at 3 AM, the on-call engineer's first task isn't fixing the problem—it's figuring out what the problem actually is. They open dashboards, check recent deployments, scan logs, and correlate signals across services.


This investigation phase often takes longer than the fix itself. AI can automate most of this work, delivering a preliminary diagnosis by the time the engineer opens their laptop.


This guide walks through setting up automated incident triage using IncidentFox.


## What We're Building


By the end of this guide, you'll have:


- AI-powered alert enrichment that gathers context automatically
- Automated root cause analysis for incoming incidents
- Slack notifications with investigation summaries
- Integration with your existing PagerDuty alerts


## Prerequisites


Before starting, you'll need:


- A Kubernetes cluster (for running IncidentFox)
- PagerDuty account with API access
- Slack workspace with permission to add apps
- Access to your monitoring stack (Prometheus, Datadog, or similar)
- Basic familiarity with kubectl and Helm


1


## Deploy IncidentFox


First, add the IncidentFox Helm repository:


```text
helm repo add incidentfox https://charts.incidentfox.ai
helm repo update
```


Create a values file for your configuration:


```text
# values.yaml
config:
llm:
provider: openai  # or anthropic, azure, local
apiKey: ${OPENAI_API_KEY}


integrations:
pagerduty:
enabled: true
apiKey: ${PAGERDUTY_API_KEY}


slack:
enabled: true
botToken: ${SLACK_BOT_TOKEN}
appToken: ${SLACK_APP_TOKEN}


prometheus:
enabled: true
url: http://prometheus:9090


storage:
type: postgresql
connectionString: ${DATABASE_URL}
```


Deploy with Helm:


```text
helm install incidentfox incidentfox/incidentfox \
--namespace incidentfox \
--create-namespace \
-f values.yaml
```


Verify the deployment:


```text
kubectl get pods -n incidentfox
```


2


## Configure PagerDuty Integration


IncidentFox needs to receive alerts from PagerDuty. Set up a webhook:


1. In PagerDuty, go to **Services** → select your service → **Integrations**
2. Click **Add Integration** → **Generic Webhook V3**
3. Set the webhook URL to your IncidentFox endpoint:


```text
https://your-incidentfox-domain/api/webhooks/pagerduty
```


1. Select which events to send (at minimum:` incident.triggered` )


Test the integration by triggering a test alert. You should see it appear in IncidentFox logs:


```text
kubectl logs -n incidentfox -l app=incidentfox -f
```


3


## Connect Your Monitoring Stack


IncidentFox needs access to your observability data to investigate incidents. Configure your data sources.


### Prometheus


If you're using Prometheus, IncidentFox can query metrics directly:


```text
# In your values.yaml
integrations:
prometheus:
enabled: true
url: http://prometheus.monitoring:9090
```


### Datadog


For Datadog, provide API credentials:


```text
integrations:
datadog:
enabled: true
apiKey: ${DATADOG_API_KEY}
appKey: ${DATADOG_APP_KEY}
site: datadoghq.com  # or datadoghq.eu
```


### Logs


Connect your log aggregator:


```text
integrations:
logs:
provider: loki  # or elasticsearch, cloudwatch
url: http://loki.monitoring:3100
```


After updating your values, upgrade the deployment:


```text
helm upgrade incidentfox incidentfox/incidentfox \
--namespace incidentfox \
-f values.yaml
```


4


## Set Up Slack Notifications


Configure IncidentFox to post investigation summaries to Slack.


### Create a Slack App


1. Go to[api.slack.com/apps](https://api.slack.com/apps) → **Create New App**
2. Choose **From scratch** , name it "IncidentFox"
3. Under **OAuth & Permissions** , add these scopes:


- ` chat:write`
- ` channels:read`
- ` groups:read`


4. Install the app to your workspace
5. Copy the **Bot User OAuth Token**


### Configure the Channel


In your values.yaml:


```text
integrations:
slack:
enabled: true
botToken: xoxb-your-bot-token
defaultChannel: "#incidents"  # Where to post summaries
```


### Test the Integration


Trigger a test alert and verify IncidentFox posts to Slack. The message should include:


- Alert summary
- Affected service
- Probable root cause (if identified)
- Links to relevant dashboards


5


## Add Your Team's Knowledge


AI SRE becomes more useful when it understands your specific environment. Add context through runbooks and historical incidents.


### Import Runbooks


If you have existing runbooks (Markdown, Confluence, Notion), import them:


```text
incidentfox kb import --source ./runbooks/
```


Or connect directly to Confluence:


```text
knowledge:
confluence:
enabled: true
url: https://your-company.atlassian.net
email: ${CONFLUENCE_EMAIL}
apiToken: ${CONFLUENCE_API_TOKEN}
spaces: ["SRE", "RUNBOOKS"]
```


### Import Historical Incidents


Past incidents help IncidentFox recognize patterns:


```text
incidentfox incidents import --source pagerduty --since 2025-01-01
```


This imports incident data including:


- Alert details
- Timeline
- Resolution notes
- Post-incident reports


The more history you provide, the better IncidentFox can identify similar issues and suggest proven fixes.


6


## Configure Triage Rules


Customize how IncidentFox handles different alert types.


Create a triage configuration:


```text
# triage-rules.yaml
rules:
- name: high-severity-immediate
match:
severity: [critical, high]
actions:
- investigate: full
- notify:
channel: "#incidents-critical"
mention: "@oncall"


- name: database-alerts
match:
service: ["postgres", "redis", "mysql"]
actions:
- investigate: full
- runbook: database-troubleshooting
- notify:
channel: "#dba-alerts"


- name: low-severity-batch
match:
severity: [low, warning]
actions:
- investigate: basic
- batch:
window: 15m
channel: "#alerts-digest"
```


Apply the configuration:


```text
kubectl create configmap triage-rules \
--from-file=triage-rules.yaml \
-n incidentfox


kubectl rollout restart deployment/incidentfox -n incidentfox
```


7


## Test the Full Flow


Now test the complete automated triage flow:


1. **Trigger a test alert** in PagerDuty (or wait for a real one)
2. **Watch the logs** to see IncidentFox receive and process it:


```text
kubectl logs -n incidentfox -l app=incidentfox -f
```


3. **Check Slack** for the investigation summary
4. **Review the analysis** in the IncidentFox UI:


```text
https://your-incidentfox-domain/incidents
```


A successful triage should show:


- Automatic data collection from your monitoring tools
- Correlation with recent changes
- Similar past incidents (if any)
- Probable root cause with confidence score
- Suggested remediation steps


8


## Tune and Iterate


After running for a few incidents:


### Review Accuracy


Check how accurate the AI's root cause analysis is. If it's often wrong about certain alert types, you may need:


- Better runbook documentation for those scenarios
- More historical incident data
- Adjusted correlation rules


### Reduce Noise


If IncidentFox is over-alerting or creating too many low-value notifications:


- Adjust triage rules to batch low-severity alerts
- Tune the similarity threshold for "related alerts"
- Add suppression rules for known flaky alerts


### Expand Coverage


Once confident in the setup:


- Add more services and alert sources
- Enable additional integrations (Grafana, Sentry, etc.)
- Consider enabling automated remediation for well-understood issues


## What Success Looks Like


After implementing automated triage, you should see:


- **Faster initial response:** Engineers wake up to a summary instead of starting from scratch.
- **Reduced investigation time:** The AI has already gathered context and tested common hypotheses.
- **More consistent triage:** Every incident gets the same thorough initial analysis, regardless of which engineer is on call.
- **Better knowledge capture:** Runbooks and past incidents become actively useful rather than forgotten documentation.


## Troubleshooting


### IncidentFox isn't receiving alerts


- Check the PagerDuty webhook configuration
- Verify network connectivity from PagerDuty to your IncidentFox endpoint
- Check the ingress/load balancer logs


### Investigation quality is poor


- Ensure monitoring integrations are working (test Prometheus queries)
- Import more historical incidents
- Add domain-specific runbooks


### Slack notifications aren't appearing


- Verify the bot token has correct permissions
- Check the bot is invited to the target channel
- Review IncidentFox logs for Slack API errors


## Next Steps


Once basic triage is working:


1. **Add more data sources:** Connect additional monitoring tools, log systems, and deployment pipelines
2. **Build team-specific agents:** Create specialized AI agents for different teams (database, payments, infrastructure) with domain-specific knowledge
3. **Enable advanced features:** Dependency mapping, predictive alerting, automated remediation
4. **Measure impact:** Track MTTR, investigation time, and engineer satisfaction to quantify the value


## Conclusion


Automated incident triage reduces the cognitive load on on-call engineers by handling the initial investigation work. Instead of manually correlating signals across tools, engineers receive a preliminary diagnosis with supporting evidence.


The setup requires some upfront work—integrations, knowledge import, rule configuration—but pays off in faster incident response and more consistent triage quality.


Start with a subset of your alerts, validate the analysis quality, and expand from there.
