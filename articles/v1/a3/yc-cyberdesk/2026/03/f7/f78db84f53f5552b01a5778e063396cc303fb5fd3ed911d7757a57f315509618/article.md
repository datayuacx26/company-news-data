---
schema_version: "1.0.0"
document_id: "f78db84f53f5552b01a5778e063396cc303fb5fd3ed911d7757a57f315509618"
company_key: "yc-cyberdesk"
company: "Cyberdesk"
source_id: "yc-cyberdesk-rss-7fc226611edf"
canonical_url: "https://cyberdesk-itcptau3z-cyberdesk.vercel.app/blog/sensitive-inputs-desktop-automation"
published_at: "2026-03-27T00:00:00+00:00"
first_seen_at: "2026-08-19T17:52:35.312646+00:00"
fetched_at: "2026-08-19T17:52:36.136936+00:00"
content_hash: "sha256:b5a9544a588da878deef1a9f91c802550e47083fc926fdaaf2883e3b65dfeead"
---

# Sensitive inputs should not become normal agent context

Desktop automation often needs to type values that should not become part of ordinary agent context. Passwords, tokens, and regulated identifiers deserve a separate path.


Cyberdesk uses sensitive inputs so teams can distinguish normal variables from values that need stronger handling.


## Separate the instruction from the secret


A workflow can say where a password should be typed without exposing the password as normal text.


```text
Click the password field in the sign-in dialog.
Type {$ehr_password}.


```


The instruction remains inspectable. The secret value stays on the sensitive input path.


That distinction seems small, but it changes how the workflow can be reviewed. An operator should be able to read the automation instructions and understand the behavior without seeing the credential itself. The workflow should say what the agent is allowed to do with the value, not reveal the value as part of the prompt.


This is especially important when workflows are edited, copied, debugged, or shared across a team. Plain inputs such as` claim_number` or` account_id` are part of the task. Sensitive inputs such as passwords, API tokens, recovery codes, and regulated identifiers need a different lifecycle.


```text
Normal input:
Use {claim_number} to find the claim row.


Sensitive input:
Use {$portal_password} only when the password field is focused.


```


The agent still has enough instruction to complete the desktop action. The secret does not become ordinary text.


## Keep outputs intentional


Sensitive inputs should not leak back through logs, screenshots, or structured outputs. A workflow can return the status of an action without returning the underlying secret.


That makes the automation easier to review and safer to operate.


The same principle applies to intermediate state. A run may need evidence that sign-in succeeded, but it does not need to store the password that was typed. A workflow may need to report that a member ID matched, but it should not include more regulated data than the downstream system requires.


Useful outputs are usually about the result of using the sensitive value:


```text
{
"login_succeeded": true,
"record_found": true,
"review_required": false
}


```


That is different from dumping the visible screen or returning every value the agent saw. The output contract should be narrow on purpose.


## Logs are product surface


Automation logs are not just developer diagnostics. In real operations they are reviewed by support teams, compliance teams, customer admins, and sometimes auditors.


That means logs should be useful without being careless. They should show which step ran, whether the action succeeded, and where the workflow stopped. They should not casually include credentials, tokens, or sensitive form values because those values happened to pass through the agent.


This is one reason sensitive input handling belongs in the runtime. If secrets are only protected by prompt wording, every downstream surface has to remember to do the right thing. If the runtime knows a value is sensitive, it can consistently keep that value out of places it does not belong.


## Security is part of the runtime


For real customers, security is not an optional wrapper around the agent. It is part of the runtime that decides how values move through the workflow.


That is why sensitive input handling belongs in the automation platform, not in a one-off prompt convention.


The runtime should answer questions a prompt cannot enforce on its own:


```text
Can this value be logged?
Can this value be sent to a model?
Can this value be copied into an output object?
Can this value be typed into a desktop field?
Who supplied the value, and for which run?


```


Those questions are boring until they are urgent. Once a team is automating credentialed systems, financial records, or healthcare workflows, they become part of the product requirements.


## Treat sensitive inputs as scoped capabilities


A useful mental model is to treat a sensitive input as a scoped capability, not as a string. The workflow is not "given the password." It is given permission to use a password for a specific action in a specific run.


That framing leads to better design. The workflow should avoid broad instructions like "here are all the credentials you might need." It should be specific about when the value is needed and what success looks like after use.


```text
1. Navigate to the portal login screen.
2. Type {$portal_username} into the username field.
3. Type {$portal_password} into the password field.
4. Submit the form.
5. Return whether login succeeded.


```


The secret is used to cross a boundary. After that, the workflow can continue with ordinary observations and structured outputs.


Sensitive input handling will never make automation risk-free. But it gives teams a practical way to reduce unnecessary exposure while still letting agents operate real desktop software.
