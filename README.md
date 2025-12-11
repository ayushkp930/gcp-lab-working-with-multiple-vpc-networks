# Lab: Working with Multiple VPC Networks (Google Cloud)

This repository documents my work on the **"Working with Multiple VPC Networks"** lab in Google Cloud.

The goal of this lab is to design and test connectivity between multiple VPC networks using **custom subnets, firewall rules, and VM instances**.

---

## Lab Objectives

- Create a **custom VPC network** (`privatenet`) with regional subnets.
- Review an existing **management VPC** (`managementnet`) and **my VPC** (`mynetwork`).
- Configure **firewall rules** to allow ICMP, SSH and RDP only where needed.
- Create multiple **Compute Engine VM instances** in different networks and regions.
- Test connectivity using `ping` and internal IPs.
- Analyze network interfaces and firewall policies using the console and Cloud Shell.

---

## VPC Networks and Subnets

### 1. `privatenet` (custom VPC)
- **Mode:** Custom
- **Routing:** Regional
- **Subnets:**
  - `privatesubnet-us` – region: `us-central1`, range: `172.16.0.0/24`
  - `privatesubnet-notus` – region: `europe-west1`, range: `172.20.0.0/20`
- No external IPs by default (private-only design).

### 2. `mynetwork`
- Existing VPC network with multiple subnets, e.g.:
  - `10.218.0.0/20` (africa-south1)
  - `10.224.0.0/20` (northamerica-south1)
  - `10.226.0.0/20` (europe-north2)
- Used to demonstrate connectivity between regions.

### 3. `managementnet`
- Custom VPC used for management tasks.
- **Subnet creation mode:** Custom
- **Dynamic routing mode:** Regional
- Used for the `managementnet-us-vm` instance.

---

## Firewall Rules

I created and reviewed several firewall rules:

- `privatenet-allow-icmp-ssh-rdp`
  - **Network:** `privatenet`
  - **Direction:** Ingress
  - **Priority:** 1000
  - **Allowed:**  
    - `icmp`  
    - `tcp:22` (SSH)  
    - `tcp:3389` (RDP)

- Default rules on the `default` and `mynetwork` VPCs:
  - `default-allow-icmp`
  - `default-allow-internal`
  - `default-allow-ssh`
  - `default-allow-rdp`

These rules were listed and verified using:

```bash
gcloud compute firewall-rules list --sort-by=NETWORK
