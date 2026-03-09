---
layout: default
title: Freemium Virtual Machines
permalink: /Freemium/virtual-machines/
---

# Freemium Virtual Machines

Most cloud VM providers charge for compute, but a few offer useful free-tier or long-term freemium options. This page lists VM offerings we have used or researched for labs, development, and small services.

## Amazon Web Services (AWS)

AWS has a useful starter VM option, but it is not permanent free capacity.

- Free-tier instance types: `t2.micro` or `t3.micro` (depending on Region)
- CPU / memory:
  - `t2.micro`: 1 vCPU, 1 GiB RAM
  - `t3.micro`: 2 vCPU, 1 GiB RAM
- Storage: EBS-backed; legacy free-tier usage includes 30 GB total EBS allowance
- Duration: 12 months for the legacy EC2 free-tier offer (not always-free)

In practice, this works fine for a small Linux VM. The main limitation is that free eligibility expires after 12 months, so it is better treated as a temporary option than a permanent free host.

## Google Cloud Platform (GCP)

Google Cloud has one of the more practical always-free VM options.

- Always Free compute: 1 non-preemptible `e2-micro` VM per month in eligible US regions
- CPU / memory: `e2-micro` shared-core profile (2 vCPU exposed, 1 GB RAM)
- Storage/network allowance:
  - 30 GB-month standard persistent disk
  - 1 GB outbound data transfer from North America to most destinations (excluding China and Australia) per month
- Duration: always free while within limits

It is small but capable, and works well for lightweight services and experimentation. It is also one of the few options that can run indefinitely under an always-free model.

One practical note from experience: the only unexpected cost we hit was enabling a static IP. That produced only a tiny charge (a few cents), but it is still worth watching.

## Oracle Cloud Free Tier

Oracle offers two useful Always Free compute paths.

### Small Intel Instance

- Shape: `VM.Standard.E2.1.Micro`
- CPU / memory: 1/8 OCPU and 1 GB RAM
- Quantity: up to 2 Always Free VM instances

This is roughly in the same class as the small AWS/GCP options for basic Linux services.

### ARM-Based Instances (Ampere)

- Shape family: `VM.Standard.A1.Flex` and bare metal Ampere options
- Always Free VM allocation: up to 4 OCPUs and 24 GB RAM total
- Storage: 200 GB total block volume for Always Free resources
- Flexibility: OCPUs/RAM can be split across multiple VMs (for example, four smaller machines)

This is an interesting free-tier option because you can divide capacity into multiple small hosts, which is useful for Docker hosts, distributed experiments, or several small services. ARM architecture can require compatibility checks for some software, but for standard Linux workloads this can be a very capable free setup.

## Other Freemium VM Options

If anyone knows other useful freemium VM offerings, feel free to add them here with notes from real-world use.
