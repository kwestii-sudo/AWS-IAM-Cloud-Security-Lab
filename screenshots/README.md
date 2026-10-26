# Screenshot evidence — AWS IAM Cloud Security Lab

This directory contains six screenshots from the AWS console and the NextWork project report. They are linked from the [main project walkthrough](../README.md), where each screenshot has an explanation of what it demonstrates.

| File | Evidence | Source |
| --- | --- | --- |
| [01-production-stop-denied.png](01-production-stop-denied.png) | The intern's production EC2 stop request was denied | AWS EC2 console test |
| [02-development-stop-allowed.png](02-development-stop-allowed.png) | The development EC2 stop operation was initiated | AWS EC2 console test |
| [03-iam-policy-simulator.png](03-iam-policy-simulator.png) | `StopInstances` was allowed and `DeleteTags` was denied in the development-tag context | IAM Policy Simulator |
| [04-development-environment-tag.png](04-development-environment-tag.png) | The development instance used the `Env=development` tag | NextWork report, page 5 |
| [05-account-alias-redacted.png](05-account-alias-redacted.png) | AWS account alias setup | NextWork report, page 8 |
| [06-iam-user-created-redacted.png](06-iam-user-created-redacted.png) | Successful IAM user creation | NextWork report, page 11 |

## Interpretation

The **production stop** was implicitly denied because no applicable identity-based policy allowed that action. The **tag modification** permission was explicitly denied by the policy. These are different IAM authorization outcomes.

The simulator screenshot shows the `development` resource-tag condition, not a separate production simulation. The production authorization failure was captured in the live EC2 test.

## Publication precautions

The screenshots were prepared with sensitive identifiers and credential-related areas cropped or redacted where relevant. Before sharing revised screenshots, inspect them for account IDs, access keys, passwords, session details, network addresses, and encoded authorization messages.

**Original guided project:** [NextWork — Cloud Security with AWS IAM](https://nextwork.ai/kelvin-wii/docs/51a5fd24-7458-58a6-8653-9b90119a0e4a).
