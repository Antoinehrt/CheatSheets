# VirtualBox - Créer une Machine Virtuelle Linux

> **Description**: Step-by-step guide to install and configure a Linux virtual machine with VirtualBox.

#VirtualBox #Linux #VM #Installation #Configuration

---

## 📋 Prerequisites

- [ ] VirtualBox installed on your system
- [ ] ISO image of the desired Linux distribution
- [ ] At least 4GB of available RAM
- [ ] Minimum 20GB of free disk space

---

## 🔧 Installation and Configuration

### 1. Install VirtualBox

1. Download and install [VirtualBox](https://www.virtualbox.org)
2. Restart the system if needed

### 2. Prepare the Linux image

1. Choose your Linux distribution
2. Download the ISO from the distribution’s official site
3. Verify file integrity (checksum recommended)

### 3. Configure the Virtual Machine

#### Create the VM

1. **New VM**: Click “New”
2. **Name and type**:
   - Name: Choose a descriptive name
   - Type: Linux
   - Version: Select the matching distribution
3. **Memory**: Allocate at least 2GB (4GB recommended)
4. **Hard disk**: Create a new virtual disk (20GB minimum)

#### Advanced settings

1. **Processors**: Assign 2+ cores if available
2. **ISO image**: Mount the ISO in the virtual CD/DVD drive
3. **Network**: Configure the network adapter (NAT by default)

### 4. Install the OS

1. **Start**: Launch the VM
2. **Install**: Follow the distribution’s installation wizard
3. **User setup**: Create a user account with a password
4. **Restart**: Reboot after installation completes

### 5. Post-Installation

#### Remove the ISO

1. Power off the VM
2. **Settings** → **Storage** → Remove the ISO from the virtual drive

#### Install Guest Additions

1. **VM menu** → **Insert Guest Additions CD image**
2. Mount the CD in the guest system
3. Run the Guest Additions installer
4. Restart the VM

![[2024-12-03_screenshot_guest_addtion.png]]

---

## 🛠️ Troubleshooting

### Missing terminal

If no console app is available:

- **Settings** → **Applications** → **Terminal** → **Open in Software** → **Install**

### Optimal performance

- Enable hardware virtualization in BIOS
- Allocate sufficient RAM
- Install Guest Additions for full integration

---

## 📚 Resources

- [VirtualBox Documentation](https://www.virtualbox.org/manual/)
- [List of Linux distributions](https://distrowatch.com/)
- [[Docker]] - Alternative for containerization
- [[Linux_Commands]] - Commands and system administration
