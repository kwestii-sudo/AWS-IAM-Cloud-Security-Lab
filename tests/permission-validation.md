# IAM Permission Validation

This document records the results of the AWS IAM lab. **Live EC2 tests** were performed while signed in as the intern IAM user; **Policy Simulator tests** were separate, non-disruptive checks.

## Test setup

- **Identity:** Intern IAM user, member of the NextWork interns group
- **Resources:** Two EC2 instances tagged `Env=development` and `Env=production`
- **Permissions:** Group-attached IAM policy; see the [reconstructed policy example](../policies/NextWorkDevEnvironmentPolicy-example.json)
- **Evidence:** Six redacted images in the [screenshots folder](../screenshots/README.md)

> **Policy provenance:** The JSON example was reconstructed from the lab documentation and simulator screenshot. It is **not** an export of the deployed AWS policy.

## Observed results

| Action and target | Test method | Expected | Observed | Evidence |
| --- | --- | --- | --- | --- |
| `ec2:StopInstances` on development EC2 | Signed-in intern, EC2 console | Allow | **Allowed**; instance entered `Stopping` | [Development stop](../screenshots/02-development-stop-allowed.png) |
| `ec2:StopInstances` on production EC2 | Signed-in intern, EC2 console | Deny | **Denied**; no matching identity-based Allow | [Production denial](../screenshots/01-production-stop-denied.png) |
| `ec2:StopInstances` with `ec2:ResourceTag/Env=development` | IAM Policy Simulator | Allow | **Allowed** | [Simulator](../screenshots/03-iam-policy-simulator.png) |
| `ec2:DeleteTags` with the simulator's development context | IAM Policy Simulator | Deny | **Denied** | [Simulator](../screenshots/03-iam-policy-simulator.png) |

## Findings

1. **Development and production behaved differently.** The intern could stop the development instance, while the production stop attempt was blocked.
2. **Production stopping was implicitly denied.** No matching policy statement granted `ec2:StopInstances` for production. This differs from an explicit `Deny`.
3. **Tag modification was explicitly denied.** The example policy blocks `ec2:CreateTags` and `ec2:DeleteTags`, helping prevent changes to the labels used for access decisions.
4. **Simulation reduced operational impact.** The live development test did change the EC2 instance state, whereas the simulator tested selected authorization decisions without stopping an instance.

## Scope and limitations

- The simulator screenshot uses the `development` tag context. It does **not** show a separate production-tag simulation; the production denial is evidenced by the live EC2 test.
- The reconstructed policy contains `ec2:*` in a conditional Allow. This is **not** a production-grade least-privilege design. Individual EC2 actions differ in resource and condition-key support and require validation.
- These results demonstrate the **tested actions**, not a comprehensive guarantee that all production changes are blocked.
- The screenshots are documentation evidence, not an automated test suite.

## Screenshot index

The [six screenshot files](../screenshots/README.md) document the environment tag, account alias, IAM user creation, both EC2 stop tests, and the IAM Policy Simulator. They are already committed and embedded in the [main README](../README.md).

**Security note:** Review screenshots and policies for credentials, AWS account identifiers, session data, and other sensitive details before any future public uploads.
