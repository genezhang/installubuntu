# How to install Ubuntu on Laptops

This doc lists the key steps in installing Ubuntu 18.04 and 20.04 on HP 14 inch laptops, and some key tricks on a Dell XPS 13 laptop.

## HP 14 laptops Models 14-dq1035cl and 14-dq1045cl

Installing Ubuntu on my two HP 14 laptops is staightforward, except for custom WIFI driver build for the RealTek rtl8821ce adapter, which took some time to figure out.

I bought them from Costco.com and they came with Windows 10 home edition. But I wiped Windows out with full Ubuntu installation.
The steps for both Ubuntu 18.04 and 20.04 are almost identical.

The key steps are as follows:

1. In case you need to re-install Windows in the future, write down the production key of the Windows by searching production key on Windows.
here is how I got the Windows production key (display for illustration only):
```
C:\Windows\System32\wmic path softwarelicensingservice get OA3xOriginalProductKey
OA3xOriginalProductKey
XX999-XXXXX-XX9XX-XXXXX-X99X9
```

2. Download Ubuntu release binary, choose desktop version from here: [Ubuntu download](https://ubuntu.com/download/desktop)

   Use checksum to verify the integrity of the binary. I used [ChecksumCalculator](http://www.tucows.com/preview/1477381/Checksum-Calculator) on Windows.

3. Create a bootable USB stick. Follow the steps here: [Create a bootable USB stick](https://ubuntu.com/tutorials/tutorial-create-a-usb-stick-on-windows#1-overview)

   There is a minor confusion point in the description of Boot selection in Rufus in the above page. Click `SELECT` button to choose iso file, there is no need to pick `FreeDOS` on the menu.

4. Power down the laptop, and when powering on, repeated press `Esc` key to get system setup menu and press F10 to get BIOS setup menu.

   You need to change two things:

   1. Disable secure boot
   2. Boot manager: change the boot order to choose boot from USB

   Exit and save the changes.

5. Now the laptop will boot from USB stick you made. You can install Ubuntu to the disk or just try out without installation. From trying out desktop,
you can also choose install.

6. When you install, choose standard installation. Most likely there is no network so other options that requires a network connection won't work. Once you finish the installation, everything should work, except for the network. So the next step is the key.

7. Use bluetooth for networking to install WIFI driver. Since there is no wire ethernet interface, the only lucky option is that Bluetooth works by default so I used it for temporary internet connection through my mobile (Android) phone bluetooth tethering.

   From phone Settings, you pair your laptop bluethooth with your mobile phone. Then from the mobile phone, enable bluetooth tethering (inside Settings, from "Mobile network" -> "Tethering & portable hotspot" -> "Bluetooth tethering"). In laptop bluetooth setting, choose "connect to internet". Verify you have internet connection with the browser.

8. The wifi adapter is from RealTek, rtl8821ce, you can verify it with the command:
```
lspci -nnk | grep -A2 0280
```

9. Follow the following steps to make and install the driver (credit to [this AskUbuntu post](https://askubuntu.com/questions/1071299/how-to-install-wi-fi-driver-for-realtek-rtl8821ce-on-ubuntu-18-04) ):
```
sudo apt-get update
sudo apt-get install --reinstall git dkms build-essential linux-headers-$(uname -r)
git clone https://github.com/tomaspinho/rtl8821ce
cd rtl8821ce
chmod +x dkms-install.sh
chmod +x dkms-remove.sh
sudo ./dkms-install.sh
```

   If the bluetooth speed is low, be patient. In my case, the download speed is only around 30+KB/s. It took a long time to finish downloading. But it worked.

   If you encounter an apt-get error saying unable to lock a file, do a `ps -ef` to list all processes to see if there is another apt process having locked a lock file. Kill that process if necessary.

   In the install process, it may ask you for a secure boot password. Type in twice.

10. Reboot the system after taking USB stick out. If necessary, enter BIOS setup to disable secure boot again. Otherwise the new installed driver will not work.

   Now you can get to setting and set wifi to connect to internet. You will be happy to see WIFI now working. A nice Ubuntu laptop!

I had some problem with wakeup from sleep on my first laptop after installation. After some struggles eventually it worked.
I believe it was because of new hardware that Ubuntu needs to get used to :-).

## Dell XPS 13 Laptop

Dell sells pre-installed Ubuntu XPS laptop, called developer edition. But it has limited config choices such as 4GiB memory only. So I went to
the Costco XPS 13 config choice with more memory. It also came with Windows Home edition installed. I made disk partition to have a dual boot system.

The overall steps are similar to above, except you don't need to install wifi driver yourself. Whatever comes with Ubuntu works.
I installed mine a couple of years ago before I saw the following posting recently (I wish I had this posting when I installed mine):

This post has good description: [How to dual boot Windows 10 and Ubuntu 18.04](https://medium.com/@pwaterz/how-to-dual-boot-windows-10-and-ubuntu-18-04-on-the-15-inch-dell-xps-9570-with-nvidia-1050ti-gpu-4b9a2901493d)

The main trick is to change the BIOS setup to make installation possible. There are a few key options in BIOS setup to make USB installation possible:
0. During power up, press F10 repeatedly to enter BIOS setup menu.
1. Set SSD from RAID mode to ACHI mode (Windows uses the RAID mode). Otherwise, Linux cannot use the SSD.
2. Disable Secure Boot.
3. Boot Manager: lower the Windows boot manager down in the list.

If you don't make it dual boot, once you have the setting correct, installation on XPS is actually simpler than HP described above thanks to working wifi.

To make it dual boot, follow the instructions on the above post. When you switch to boot Windows, you need to change BIOS settings back. So it's not very convenient.

----
Hope the above tips help, and you will enjoy your Ubuntu. Let me know if you'd like to improve this doc.

