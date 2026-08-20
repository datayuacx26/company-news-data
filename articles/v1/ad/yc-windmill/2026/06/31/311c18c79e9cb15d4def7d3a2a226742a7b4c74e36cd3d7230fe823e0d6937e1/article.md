---
schema_version: "1.0.0"
document_id: "311c18c79e9cb15d4def7d3a2a226742a7b4c74e36cd3d7230fe823e0d6937e1"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-6969ef4af7f4"
canonical_url: "https://www.windmill.dev/changelog/ssh-execution"
published_at: "2026-06-10T00:00:00+00:00"
first_seen_at: "2026-07-25T01:07:57.288074+00:00"
fetched_at: "2026-07-28T21:11:25.860154+00:00"
content_hash: "sha256:109941369bf504adc20a89f7f0e4dd45029a32f0b1a0c28f0b18da257f3defe7"
---

# Run bash scripts on a remote SSH host

### [Run bash scripts on a remote SSH host](https://www.windmill.dev/changelog/ssh-execution)


Scripts


[Enterprise](https://www.windmill.dev/pricing)


[v1.721.0](https://github.com/windmill-labs/windmill/releases/tag/v1.721.0)


[Docs](https://www.windmill.dev/docs/advanced/ssh_execution)


Bash scripts starting with a` #ssh <resource_path>` directive now run on the remote host described by the referenced` ssh_target` resource, for jump/utility nodes where no worker can be placed. Full parity with local bash jobs - typed positional args, structured result, live streamed logs, cancellation and remote exit-code propagation. Enterprise feature, off by default (` ssh_execution_enabled` instance setting). Agent workers remain the recommended way to run code in isolated environments.


#### New features


- Add \`#ssh <resource_path>\` as the first comment line of a bash script to run it on the remote host of an \`ssh_target\` resource.
- Full parity with local bash jobs: typed args, result collection (result.json > result.out > last stdout line), live logs, cancellation, remote exit code fails the job.
- Dynamic target with \`#ssh $<arg_name>\`: the target resource path is supplied as a job argument at call time, resolved through the runner resource permissions.
- Host-key pinning enforced when the resource sets \`host_pubkey\`; trust-on-first-use only with explicit \`accept_unknown_host\`.
- Enterprise-gated and off by default: enable the \`ssh_execution_enabled\` instance setting as a superadmin.
- A userland wrapper (\`ssh_exec.sh\` / \`ssh_exec.py\`) with the same SSH mechanics is available without a license in examples/usecase/ssh-execution-wrapper.
