### 🧪 4. Testing & Validation (`docs/testing-and-validation.md`)

```markdown
# Validation and Troubleshooting Log

## 1. Bastion & App-A Connectivity Test
* **Command:** `ssh ewb-app-a`
* **Result:** Successfully bypassed the public bastion via ProxyJump and landed on Amazon Linux 2023 instance `10.10.2.10`.
* **Verification:** Prompt successfully changed to `[ec2-user@ip-10-10-2-10 ~]$`.

## 2. Inter-VPC Routing / Port Verification
* **Objective:** Verify TCP communication across peered VPCs to `App-B` (`10.20.1.10`).
* **Command:** `/dev/tcp/10.20.1.10/22` socket check / port verification.
* **Result:** Connection succeeded, confirming that route tables and Security Group rules (`ewb-sg-app-b`) permit secure cross-VPC traffic flow.