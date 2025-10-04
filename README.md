# Homelab

Documentation and configuration for my homelab setup, including Docker services, Proxmox VM configurations, and security tools for cybersecurity research.

## My Homelab Project

Welcome to my homelab project repository! This repository documents the setup, configuration, and management of my homelab environment, which includes various services running on a Proxmox VM with Docker containers. The homelab is designed for personal use, experimentation, **cybersecurity research**, and self-hosting various applications.

---

## Overview

### Homelab Infrastructure

- **Host Machine**: Hosted on a **Proxmox VE** environment, running Debian 12 (Bookworm).
- **Hardware**:
  - **CPU**: **AMD Ryzen 9 7950X** (16-Core, 4.5 GHz)
  - **CPU Cooler**: Noctua NH-U12A chromax.black
  - **Motherboard**: MSI MAG B650M MORTAR WIFI Micro ATX
  - **Memory**: **96 GB** (2 x 48 GB) DDR5-6400 CL32 G.Skill Ripjaws S5
  - **Video Card**: Sparkle ECO Arc A310 4 GB
  - **Power Supply**: Corsair SF750 (2024) 750 W 80+ Platinum
  - **Case**: Fractal Design Node 804
  - **HBA**: LSI Logic Controller Card 9300-8i (12Gb/s)
  - **Storage**:
    - **OS/High-Speed**: **Samsung 990 Pro 2 TB** M.2 NVMe SSD (for Proxmox and high-priority Docker containers)
    - **NAS/Media Storage**: Three **Seagate IronWolf Pro NAS 24 TB** HDDs (Total 72 TB raw storage, managed via HBA)

---

### Docker Containers

- **Minecraft Server (x2)**: Set up using Docker Compose with RCON connectivity for remote management (`atbcraft_default` and `cobblemon_default` networks).
- **Jellyfin**: A media server for streaming content, accessible via **Tailscale VPN**.
- **Filebrowser**: A web-based file manager for accessing files stored on the NAS drives.
- **Tailscale**: A **VPN solution** for secure remote access to the homelab without exposing services to the broader internet.
- **Immich**: A self-hosted photo and video backup solution, designed for automatic uploads from mobile devices, with **AI-powered search and tagging** for easy organization.
- **Caddy**: An alternative reverse proxy to Traefik/Nginx-Proxy-Manager. Currently used for its simpler syntax and **automatic HTTPS/TLS certificate provisioning** for containers on the `caddy_network`.

---

### Virtual Machines

- **Debian12 VM**: A VM solely dedicated to running and managing Docker containers, primarily through the Portainer WebUI.
- **REMnux VM**: A specialized VM for **malware analysis**. REMnux is a Linux distribution tailored for reverse-engineering and analyzing malicious software, using tools like Ghidra, IDA Free, and various static and dynamic analysis utilities.
- **Kali VM**: A VM for **penetration testing and ethical hacking**, equipped with tools like Metasploit, Burp Suite, and Nmap for comprehensive security assessments.
- **Arch Linux**: A bare-bones, highly customizable Linux distribution chosen for its minimalism, lack of preinstalled tools, and common use for "ricing" (heavy customization).

---

### Network Configuration

- **Tailscale**: Configured with IP forwarding and LAN subnet advertising. Provides remote access to the NAS and Jellyfin server through the Tailscale IP address.
- **Headscale**: A selfhosted alternative to Tailscale, based on WireGuard. This is a planned change to become the primary remote access VPN.
- **Pi-hole**: A **network-wide ad-blocking and DNS sinkhole**. It will be configured as the primary DNS server for the network to block advertisements, trackers, and malicious domains at the network level.
- **Nginx Proxy Manager (NPM)**: A reverse proxy management tool that simplifies exposing internal services securely. It is used to manage hostnames, automatically obtain **Let's Encrypt SSL certificates**, and forward external requests to the correct internal Docker container or VM.
- **Hostname Management**: Hostnames of Docker containers can be directly edited through Portainer's Network settings.
- **VLAN Management**: (tbd) Planned for management through a switch (model to be determined), setting 4 distinct VLANs: **Untrusted LAN**, **Trusted LAN**, **IOT LAN**, and **Outward Facing Service LAN**.

---

### Storage Management

- **Backup Solutions**: Proxmox backups are managed by a Docker container running **PBS (Proxmox Backup Server)**. Utilizes a dedicated storage volume (from the 66 TB pool) with weekly backups.
- **Redundancy**: Dedicated backup server associated with the Minecraft servers to ensure no loss of progress and data redundancy in case of file corruption.

---

### Docker Setup

- **Directory Structure**: Docker containers are organized within `/user/docker_projects`, ensuring a clean and manageable environment.
- **Key Configurations**:
  - **Custom Modded Minecraft Server (x2)**: Managed with Docker Compose for easy deployment, ensuring connectivity by placing all services on the same Docker network (`atbcraft_default` and `cobblemon_default`).
  - **Jellyfin**: Managed via Filebrowser NAS, with a large media library hosted and remotely accessible via Tailscale VPN.

---

## Future Plans

- **Internal DNS**: Setting local DNS records with **Pi-hole** for ease of use of services without exposing outward with NPM.
- **VPN Migration**: Exploring and migrating to **HeadScale** as a self-hosted alternative to Tailscale for network access.
- **VLAN Implementation**: Setting up the planned 4-tiered VLAN structure for network segmentation and enhanced security.

---

## Challenges and Solutions

- **Networking**: Resolved initial RCON connectivity issues by ensuring services were on the same Docker network.
- **Storage Resizing**: Successfully resized partitions within the VM after resizing the host partition.
- **Remote Access**: Achieved secure remote access to multiple services using **Tailscale**.
