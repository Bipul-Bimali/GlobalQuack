# GlobalQuack

# Remote Rubber Ducky with Raspberry Pi 4

### Overview
This project transforms a Raspberry Pi 4 into a self-powered, remotely accessible rubber ducky using the Type-C interface to send keystrokes. Unlike traditional rubber duckies that require physical proximity, this solution enables global keystroke execution via SSH and VPN.

### Features
- **Global Accessibility**: Send keystrokes from anywhere in the world, thanks to SSH and VPN.
- **Self-Powered**: Utilizes Raspberry Pi 4's Type-C interface for power and data transfer.
- **DuckScript Conversion**: A web application for converting Hack5 Rubber Ducky scripts into Python code for seamless remote execution.

### Setup Instructions

#### Prerequisites
1. Raspberry Pi 4 with Debian OS installed.
2. Active VPN subscription for secure remote access.
3. SSH service enabled on the Raspberry Pi.

#### Steps
1. **Install Dependencies**:
   ```bash
   sudo apt update
   sudo apt install python3 ssh
