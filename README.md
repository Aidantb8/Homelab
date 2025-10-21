# Homelab

Documentation and configuration for my homelab setup, including Docker services, Proxmox VM configurations, and security tools for cybersecurity research.

## My Homelab Project

Welcome to my homelab project repository! This repository documents the setup, configuration, and management of my homelab environment, which includes various services running on Docker Containers via several VM's within a baremetal Proxmox installation. The homelab is designed for personal use, experimentation, **cybersecurity research**, and self-hosting various applications.

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
- **Pi-hole**: A network-wide ad-blocking and DNS sinkhole for traffic filtering.
- **Nginx Proxy Manager (NPM)**: A reverse proxy management tool for secure external access. It is currently running alongside **Caddy**, which is used for its simpler syntax and **automatic HTTPS/TLS certificate provisioning** for containers on the `caddy_network`.
- **n8n**: A powerful workflow automation tool used to connect and automate various services across the homelab and the web.
- **Arr Stack (Sonarr, Radarr, Lidarr)**: A suite of applications for managing and automating media downloading and organization for TV shows, movies, and music.
- **Jellyseerr**: A request management and media discovery tool that integrates with Jellyfin and the Arr Stack, allowing users to easily request new content.
- **Memos**: A self-hosted, lightweight note-taking and knowledge-base service for quick personal documentation.
- **VS Code Server**: A backend service that enables access to the full Visual Studio Code experience through a web browser, used for remote development and configuration editing.
- **Uptime Kuma**: A self-hosted monitoring tool to track the uptime and status of all homelab services with notifications.

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
- **VLAN Management**: (tbd) Planned for management through a switch (model to be determined), setting 4 distinct VLANs: **Untrusted VLAN**, **Trusted VLAN**, **IOT VLAN**, and **Outward Facing Service (Server) VLAN**.

---

### Storage Management

- **Backup Solutions**: Proxmox backups are managed by a VM running **PBS (Proxmox Backup Server)**. Utilizes a dedicated storage volume (from the 72 TB pool) with weekly backups.
- **Redundancy**: Dedicated backup server associated with each Minecraft server to ensure no loss of progress and data redundancy in case of file corruption.

---

### Docker Setup

- **Directory Structure**: Docker containers are organized within `/user/docker_projects`, ensuring a clean and manageable environment.
- **Key Configurations**:
  - **Custom Modded Minecraft Server (x2)**: Managed with Docker Compose for easy deployment, ensuring connectivity by placing all services on the same Docker network (`atbcraft_default` and `cobblemon_default`).
  - **Jellyfin**: Primarily managed through the Arr Stack, which is parsed through filebrowser for manual file additions if necessary. Utilizes atomic moves in order for the file to appear in two places at once. 

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
