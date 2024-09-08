# Homelab

Documentation and configuration for my homelab setup, including Docker services, Proxmox VM configurations, and security tools for cybersecurity research.

## My Homelab Project

Welcome to my homelab project repository! This repository documents the setup, configuration, and management of my homelab environment, which includes various services running on a Proxmox VM with Docker containers. The homelab is designed for personal use, experimentation, cybersecurity research, and self-hosting various applications.

## Overview

### Homelab Infrastructure

- **Host Machine**: Hosted on a Proxmox VE environment, running Debian 12 (Bookworm).
  
- **Hardware**:
  - **CPU**: Intel i9 9750H
  - **GPU**: NVIDIA 1660 Ti
  - **RAM**: 16GB (Soon 32GB)
  - **Storage**:
    - 256GB SSD: Dedicated to the Proxmox Virtual Environment Hypervisor
    - 1TB SSD: Dedicated for system processes and Docker containers
    - 2TB HDD (`/dev/sdb1`): Used as a NAS for Filebrowser and media storage
    - 5TB External HDD (`/dev/sdb2`): Secondary NAS and media storage
    - 16TB HDD: Dedicated to the Proxmox Backup Server

### Docker Containers

- **Minecraft Server**: Set up using Docker Compose with RCON connectivity for remote management.
- **Jellyfin**: A media server for streaming content, accessible via Tailscale VPN.
- **Filebrowser**: A web-based file manager for accessing files stored on the NAS (`/dev/sdb1`).
- **Tailscale**: A VPN solution for secure remote access to the homelab without exposing services to the broader internet.

### Virtual Machines

- **REMnux VM**: A specialized VM for malware analysis. REMnux is a Linux distribution tailored for reverse-engineering and analyzing malicious software. This VM uses tools like Ghidra, IDA Free, and various static and dynamic analysis utilities.
- **Kali VM**: A VM for penetration testing and ethical hacking, equipped with tools like Metasploit, Burp Suite, and Nmap for comprehensive security assessments.

### Network Configuration

- **Tailscale**: Configured with IP forwarding and LAN subnet advertising. Provides remote access to the NAS and Jellyfin server through the Tailscale IP address.
- **Hostname Management**: Hostnames of Docker containers can be directly edited through Portainer's Network settings.

### Storage Management

- **Backup Solutions**: Proxmox backups are managed by a Docker container running PBS (Proxmox Backup Server). Utilizes a dedicated 16TB HDD with weekly backups.

### Docker Setup

- **Directory Structure**: Docker containers are organized within `/root/docker_projects`, ensuring a clean and manageable environment.

- **Key Configurations**:
  - **Minecraft Server**: Managed with Docker Compose for easy deployment. Ensures connectivity by placing all services on the same Docker network (`atbcraft_default`).
  - **Jellyfin**: Managed via Filebrowser NAS, with ~7TB of media hosted and remotely accessible via Tailscale VPN.

## Future Plans

- **Security Enhancements**: Integrating Pi-hole with Traefik for ad-blocking and reverse proxy management.

## Challenges and Solutions

- **Networking**: Resolved initial RCON connectivity issues by ensuring services were on the same Docker network.
- **Storage Resizing**: Successfully resized partitions within the VM after resizing the host partition.
- **Remote Access**: Achieved secure remote access to multiple services using Tailscale.

