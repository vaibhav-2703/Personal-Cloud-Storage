# Personal-Cloud-Storage

## Overview

This project aims to create a personal cloud storage server for a firm, solving issues such as accessing important files after hours and preventing data loss due to corrupted or multiple drives.

## Problem Statement

The firm faced the following challenges:
- **Accessing Files After Hours**: Employees needed to retrieve important files from office computers after office hours or late at night.
- **Data Loss**: Files were occasionally lost due to drive corruption or issues with multiple drives.

## Solution

The solution involves setting up a personal cloud storage server with the following steps:

1. **Initial Setup**:
   - Used an old PC from the office.
   - Installed Ubuntu Server 24.04 LTS.
   - Set up Nextcloud for file storage and management.
   - Installed MariaDB as the database server.
   - Configured Apache or Nginx as the web server.

2. **Enabling Remote Access**:
   - Initially, the server was only accessible within the local network.
   - The ISP provided a dynamic IP, and the router/modem did not support port forwarding.
   - Used Cloudflare Tunnel to enable external access without requiring VPN or similar services.

3. **Optimization and Improvements**:
   - **Docker Integration**:
     - Transitioned to using Docker for containerization.
     - Implemented Docker Compose to manage Nextcloud, Cloudflare, MySQL, and the web server.
   - **Enhanced Server Management**:
     - Switched to TrueNAS for a user-friendly GUI.
     - Configured TrueNAS to be accessible outside the local network using Cloudflare Tunnel, allowing remote server management.

## Results

This project successfully provided the firm with:
- A reliable personal cloud storage solution.
- Remote access and management capabilities.
- Improved server performance and management.

## Future Enhancements

Potential future improvements include further performance optimization and exploring additional features in TrueNAS.

## Installation and Configuration

For detailed setup instructions, refer to:
- [INSTALLATION.md](INSTALLATION.md)
- [CONFIGURATION.md](CONFIGURATION.md)

## Contributing

Interested in contributing? See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## Project Status

Track the current state of the project and planned future work in [STATUS.md](STATUS.md).

## References

- Nextcloud setup guide: [LinuxLearnTV](https://www.learnlinux.tv/complete-walkthrough-for-installing-nextcloud-on-ubuntu-24-04/)
- Official Nextcloud AIO Docker Compose file: [Nextcloud AIO](https://github.com/nextcloud/all-in-one/blob/main/compose.yaml)
