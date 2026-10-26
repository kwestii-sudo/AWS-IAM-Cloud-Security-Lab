# IAM Permission Validation

This records **observed** results from the NextWork AWS IAM lab. The example JSON policy is reconstructed from the simulator screenshot, **not exported from AWS**.

## Setup

- Identity: test intern IAM user, member of an interns IAM group
- Two EC2 instances tagged `Env=development` and `Env=production`
- Policy: [NextWorkDevEnvironmentPolicy-example.json](../policies/NextWorkDevEnvironmentPolicy-example.json)

## Observed tests

| Test | Method | Expected | Observed |
| --- | --- | --- | --- |
| Stop development instance | Intern session in EC2 console | Allow | **Allowed** — EC2 transitioned to `Stopping` |
| Stop production instance | Intern session in EC2 console | Deny | **Denied** — no identity-based policy allowed `ec2:StopInstances` |
| StopInstances with development tag | IAM Policy Simulator | Allow | **Allowed** |
| DeleteTags | IAM Policy Simulator | Deny | **Denied** |

## Interpretation and limitations

- The production stop was **implicitly denied**, not blocked by an explicit `Deny` on that operation.
- `ec2:CreateTags` and `ec2:DeleteTags` were expressly denied.
- The simulator screenshot used `ec2:ResourceTag/Env=development`. There is **no evidence here of a separate simulator run** using `Env=production`; production denial came from the live EC2 console test.
- The example policy uses `ec2:*`, which is broader than a production-grade least-privilege policy. Replace it with necessary actions and verify condition-key support before real deployment.
- The live development test changed the EC2 instance state. For routine permission checks, prefer the simulator.

## Screenshot evidence

Three redacted screenshots were prepared for this project but are not yet uploaded here:

1. `screenshots/01-production-stop-denied.png` — authorization failure from production stop
2. `screenshots/02-development-stop-allowed.png` — successful development stop initiation
3. `screenshots/03-iam-policy-simulator.png` — allowed StopInstances / denied DeleteTags

Review images for account IDs, usernames, sign-in links, public network information and tokens before publishing.
