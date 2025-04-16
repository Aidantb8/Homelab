# Homelab

Documentation and configuration for my homelab setup, including Docker services, Proxmox VM configurations, and security tools for cybersecurity research.

## My Homelab Project

Welcome to my homelab project repository! This repository documents the setup, configuration, and management of my homelab environment, which includes various services running on a Proxmox VM with Docker containers. The homelab is designed for personal use, experimentation, cybersecurity research, and self-hosting various applications.

---

## Overview

### Homelab Infrastructure

- **Host Machine**: Hosted on a Proxmox VE environment, running Debian 12 (Bookworm).
- **Hardware**:
  - **CPU**: Intel i9 9750H
  - **GPU**: NVIDIA 1660 Ti
  - **RAM**: 16GB (Soon 32GB)
  - **Storage**:
    - 256GB SSD: Dedicated to the Proxmox Virtual Environment Hypervisor
    - 1TB SSD: Dedicated for system processes and high priority Docker containers
    - 2TB HDD (`/dev/sdb1`): Secondary NAS and media storage, also used for low priority Docker containers
    - 4TB HDD ('/dev/sdb3'): Secondary NAS and media storage
    - 5TB External HDD (`/dev/sdb2`): Primary NAS and media storage drive
    - 16TB HDD: Dedicated to the Proxmox Backup Server

---

### Docker Containers

- **Minecraft Server**: Set up using Docker Compose with RCON connectivity for remote management.
- **Jellyfin**: A media server for streaming content, accessible via Tailscale VPN.
- **Filebrowser**: A web-based file manager for accessing files stored on the NAS (`/dev/sdb*`).
- **Tailscale**: A VPN solution for secure remote access to the homelab without exposing services to the broader internet.
- **Immich**: A self-hosted photo and video backup solution, designed for automatic uploads from mobile devices, with AI-powered search and tagging for easy organization.
- **Caddy**: An alternative to Traefik and Nginx-Proxy-Manager. While I'm considering going back to using Traefik with LetsEncrypt + CloudFlare DNS for the sake of learning, I'm currently using Caddy just to get the intended result. It uses a much simpler and uses more intuitive syntax that creates automatic HTTPS provisions / TLS certifications for any Docker container included in the Docker network ('caddy_network')

---

### Virtual Machines

- **Debian12 VM**: A VM solely dedicated to running and managing Docker containers, primarily through the Portainer WebUI.
- **REMnux VM**: A specialized VM for malware analysis. REMnux is a Linux distribution tailored for reverse-engineering and analyzing malicious software. This VM uses tools like Ghidra, IDA Free, and various static and dynamic analysis utilities.
- **Kali VM**: A VM for penetration testing and ethical hacking, equipped with tools like Metasploit, Burp Suite, and Nmap for comprehensive security assessments.
- **Arch Linux**: Arch is a bare-bones, independently developed linux distro, known for it's lack of 'ease of use' tools. Due to it's minimalismm and lack of preinstalled tools or configurations, it's highly customizable and a common choice for "ricing" or heavily customizing a linux distribution. 

---

### Network Configuration

- **Tailscale**: Configured with IP forwarding and LAN subnet advertising. Provides remote access to the NAS and Jellyfin server through the Tailscale IP address.
- **Headscale**: A selfhosted alternative to Tailscale, also based on WireGuard. I'm planning on changing to this as my primary VPN for accessing my network remotely, but while I configure it I'm keeping the Tailscale service running. 
- **Hostname Management**: Hostnames of Docker containers can be directly edited through Portainer's Network settings.
- **VLAN Management**: (tbd) Managed through _ , setting 4 distinct VLANs. The VLANS consist of: Untrusted LAN, Trusted LAN, IOT LAN, and Outward Facing Service LAN.

---

### Storage Management

- **Backup Solutions**: Proxmox backups are managed by a Docker container running PBS (Proxmox Backup Server). Utilizes a dedicated 16TB HDD with weekly backups.

---

### Docker Setup

- **Directory Structure**: Docker containers are organized within `/user/docker_projects`, ensuring a clean and manageable environment.
- **Key Configurations**:
  - **Custom Modded Minecraft Server**: Managed with Docker Compose for easy deployment. Ensures connectivity by placing all services on the same Docker network (`atbcraft_default`).
  - **Additional Custom Modded Minecraft Server**: Similar to the previous Minecraft Server, it is also Managed with Docker Compose using itzg/docker-minecraft-server, this time using the Docker network ('cobblemon_default')
  - **Backup Server Associated with Minecraft Server** A dedicated backup server ensuring no loss of progress as well as data redundancy in case of file corruption. Also on the same Docker network ('atbcraft_default')
  - **Jellyfin**: Managed via Filebrowser NAS, with ~7TB of media hosted and remotely accessible via Tailscale VPN.

---

## Future Plans

- **Security Enhancements**: Integrating Pi-hole with Traefik for ad-blocking and reverse proxy management. Additionally, exploring HeadScale as a self-hosted alternative to Tailscale.

---

## Challenges and Solutions

- **Networking**: Resolved initial RCON connectivity issues by ensuring services were on the same Docker network.
- **Storage Resizing**: Successfully resized partitions within the VM after resizing the host partition.
- **Remote Access**: Achieved secure remote access to multiple services using Tailscale.
