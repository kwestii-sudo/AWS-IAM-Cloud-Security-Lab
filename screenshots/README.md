# AWS IAM Lab — Screenshot Evidence

This directory documents the six **redacted evidence images** prepared from the user's AWS console captures and 15-page NextWork PDF export. The image binaries are ready for upload from the accompanying ZIP but **have not yet been added to this repository**.

| Filename to upload | Source | What it proves |
| --- | --- | --- |
| `01-production-stop-denied.png` | EC2 console capture | Production `ec2:StopInstances` request denied (no applicable Allow) |
| `02-development-stop-allowed.png` | EC2 console capture | Development stop operation successfully initiated |
| `03-iam-policy-simulator.png` | IAM Policy Simulator capture | StopInstances allowed with development tag; DeleteTags denied |
| `04-development-environment-tag.png` | NextWork PDF, page 5 | EC2 resource tag `Env=development` |
| `05-account-alias-redacted.png` | NextWork PDF, page 8 | IAM account alias setup with account ID obscured |
| `06-iam-user-created-redacted.png` | NextWork PDF, page 11 | Successful IAM user creation; credentials panel cropped out |

## Evidence security

The images have been cropped/redacted to exclude account IDs, EC2 instance IDs, IAM identity details, public DNS identifiers, encoded authorization failure messages and credential retrieval fields where relevant. Treat AWS sign-in URLs, credentials, account IDs, and session data as sensitive.

## Interpreting the results

The production stop was **implicitly denied**, because no matching Allow applied. In contrast, the policy used an explicit deny to protect EC2 tag modification. The simulator only shows the `Env=development` context; the production denial is from the actual console test.

**Source report:** [NextWork Cloud Security with AWS IAM](https://nextwork.ai/kelvin-wii/docs/51a5fd24-7458-58a6-8653-9b90119a0e4a?utm_source=linkedin).
