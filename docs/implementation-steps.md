# Implementation Guide: Secure Cross-VPC Peering Lab

## Phase 1: VPC & Subnet Creation
1. Created **VPC-A** with CIDR `10.10.0.0/16` in AWS Region `ap-southeast-2`.
2. Created **VPC-B** with CIDR `10.20.0.0/16` in the same region.
3. Provisioned subnets:
   * `ewb-subnet-public-a` (`10.10.1.0/24`)
   * `ewb-subnet-private-a` (`10.10.2.0/24`)
   * `ewb-subnet-private-b` (`10.20.1.0/24`)

## Phase 2: Internet Gateway & Route Tables
1. Attached an Internet Gateway (`IGW`) to `VPC-A`.
2. Created a public route table (`ewb-rt-public`) routing `0.0.0.0/0` to the IGW.
3. Created private route tables (`ewb-rt-a-private` and `ewb-rt-b-private`).

## Phase 3: VPC Peering Configuration
1. Initiated a peering connection from `VPC-A` (Requester) to `VPC-B` (Accepter).
2. Accepted peering connection `pcx-011a8f0c6eada679b` in `VPC-B`.
3. Updated route tables:
   * In `ewb-rt-a-private`, added route for `10.20.0.0/16` targeting `pcx-011a8f0c6eada679b`.
   * In `ewb-rt-b-private`, added route for `10.10.0.0/16` targeting `pcx-011a8f0c6eada679b`.

## Phase 4: SSH Configuration & Access
Configured local Windows SSH client (`C:\Users\KIIT\.ssh\config`) for streamlined multi-hop jumping:
```text
Host ewb-bastion
    HostName <my_ip>
    User ec2-user
    IdentityFile ~/.ssh/ewb-lab.pem
    IdentitiesOnly yes

Host ewb-app-a
    HostName 10.10.2.10
    User ec2-user
    IdentityFile ~/.ssh/ewb-lab.pem
    IdentitiesOnly yes
    ProxyJump ewb-bastion
Host ewb-app-b
    HostName 10.20.1.10
    User ec2-user
    IdentityFile ~/.ssh/ewb-lab.pem
    IdentitiesOnly yes
    ProxyJump ewb-bastion
