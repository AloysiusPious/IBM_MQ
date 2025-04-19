# IBM MQ Explorer Installation and Setup

This section outlines the steps to install and set up IBM MQ Explorer on a Linux system, and also how to create and manage Queue Managers.
---

## Prerequisites
Before starting the installation, make sure you have the following:

- A Linux machine with appropriate permissions.
- IBM MQ Explorer binary: [Download IBM MQ Explorer 9.4.2.0](https://www.ibm.com/support/pages/mq-explorer).

---

## 1. Installing IBM MQ Explorer

Follow these steps to install IBM MQ Explorer on your system:

### Step 1: Move the MQ Explorer Package

First, move the `9.4.2.0-IBM-MQ-Explorer-LinuxX64.tar.gz` binary to the desired directory:

```bash
sudo mv 9.4.2.0-IBM-MQ-Explorer-LinuxX64.tar.gz /opt/softwares/
cd /opt/softwares/
```
## Step 2: Extract the Package
Next, extract the downloaded tarball file:

```bash
tar -zxvf 9.4.2.0-IBM-MQ-Explorer-LinuxX64.tar.gz
cd MQExplorer/
```
## Step 3: Set Permissions
Make sure the binaries have the appropriate execute permissions:

```bash
chmod 755 *
```
## Step 4: Install IBM MQ Explorer

Run the Setup.bin file to begin the installation in console mode:

```bash
sudo ./Setup.bin -i console
```

## Step 5: Restart the System
After the installation is complete, restart your system:

```bash
sudo reboot now
```
