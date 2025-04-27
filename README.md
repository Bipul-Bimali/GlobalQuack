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


## 🛠️ Setup Instructions

### 1. Modify the `/boot/config.txt` File

Enable the `dwc2` overlay for USB gadget support:

```bash
sudo nano /boot/firmware/config.txt
```

### 2.Add  Following lines to the file
``` txt
dtoverlay=dwc2
```
### 3.Edit the cmdline.txt file  to add gadget mode driver 
#### Add the following text  after rootwait
```txt
modules-load=dwc2,g_serial
```
### 3. Create a HID Configuration
####  Open  /etc/modules and 



### 4. Create a script to initiate  HID mode 
Create a Bash script to start make it a keyboard
```bash
sudo nano /usr/bin/setup_usb_keyboard.sh
```
The bash shell is  to use is :-
```bash 
#!/bin/bash
cd /sys/kernel/config/usb_gadget/
mkdir -p pi_keyboard
cd pi_keyboard

echo "0x1d6b" > idVendor  # Linux Foundation
echo "0x0104" > idProduct # USB Keyboard
echo "0x0100" > bcdDevice
echo "0x0200" > bcdUSB

mkdir -p strings/0x409
echo "Raspberry Pi HID Keyboard" > strings/0x409/product

mkdir -p configs/c.1
mkdir -p functions/hid.usb0
echo 1 > functions/hid.usb0/protocol
echo 1 > functions/hid.usb0/subclass
echo 8 > functions/hid.usb0/report_length
echo -ne '\x05\x01\x09\x06\xa1\x01\x05\x07\x19\x00\x29\x65\x15\x00\x25\x65\x75\x08\x95\x06\x81\x00\xc0' > functions/hid.usb0/report_desc

ln -s functions/hid.usb0 configs/c.1/
ls /sys/class/udc > UDC
```
 Make the file executeable
```bash 
sudo chmod +x /usr/bin/setup_usb_keyboard.sh
```
Run the bash script as sudo to  start using device a keyboard

### 5.Automate the initialization
run ```crontab -e``` as sudo and add the following lines;
``` txt
@reboot /usr/bin/setup_usb_keyboard.sh
@reboot openvpn /Path/to/vpnfile # I was using openvpn for VPN  access
```
### 6. setup ssh server to gain shell and send  scripts 

### 7. Use Python script as sudo   to send keystrokes
```Python
with open("/dev/hidg0", "rb+") as f:
    f.write(b'\x00\x00\x04\x00\x00\x00\x00\x00')  # Press 'A'
    f.write(b'\x00\x00\x00\x00\x00\x00\x00\x00')  # Release 'A'
    f.write(b'\x00\x00\x28\x00\x00\x00\x00\x00')  # Press 'Enter'
    f.write(b'\x00\x00\x00\x00\x00\x00\x00\x00')  # Release 'Enter'
```
## The webapplication to convert Ducky script to Python code will be available shortly on [DuckyToPython](http://ahak.bipulbimali.com.np/DuckyToPython.php)

### Community Contributions

Your input is valuable! If you have any suggestions, ideas, or feedback to improve this project, feel free to contribute. Here’s how you can get involved:
1. **Open an Issue**: Have a feature request or found a bug? Submit an issue [here](https://github.com/Bipul-Bimali/GlobalQuack/issues).
2. **Fork and Improve**: Fork the repository, make your improvements, and open a pull request.
3. **Share Ideas**: Connect with me directly via GitHub Discussions or on social media.

Together, we can make this project even better! 🚀
