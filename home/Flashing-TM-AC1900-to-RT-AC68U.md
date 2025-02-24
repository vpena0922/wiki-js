# Flashing TM-AC1900 to RT-AC68U

**WARNING**: Do not update to new Asus or Merlin builds without executing code for fixing mtd5 partition.

**How to flash AC1900 to AC68U:**

1. Install WinSCP, Putty, and 7zip. Install the [Asus Restore Utility](https://mega.nz/file/MX83jJKS#SA3VvN3sFjh2o9OBsrXE2hg9Rob-m6iPsWfb4Rmgk70), [mirror](http://www.filedropper.com/utrtac68u4292).
2. Create a folder on your desktop, name it “router”.
3. Download [this file](https://mega.nz/file/0PVhTSJR#RkG00pID5j3K4_Lt8qpW5tOoen0gHEljlhBjYpw3K6o) and extract it to the newly made router folder. It contains a firmware to downgrade to, a firmware to upgrade to, and mtd-write.
4. Set static IP for PC. *start > run > ncpa.cpl > double click Ethernet > properties > IPV4 >* *IP: 192.168.29.5* *Subnet: Default (255.255.255.0)* *Gateway: 192.168.29.1*
5. Place router into Recovery/Restore mode *Hold reset button 10 seconds* *Power off router (keep holding reset)* *Wait 10 seconds, keep holding reset* *Power on router holding reset for 10 more seconds*
6. Go to 192.168.29.1 in a web browser *If Mini-CFE webpage won’t load use Asus Restore Utility If having trouble with this step perform NVRam Reset and try #6 again (see step #20)*
7. Flash TM-AC1900_3.0.0.4_376_1703-g0ffdbba.trx
8. Wait for reboot <5 mins – WiFi lights on router will turn on when boot is complete (WAIT! It will upload and reboot!)
9. Log in to router (admin:password)
10. Go to Administration > System > Enable SSH > Yes > Apply Ezlink: [http://192.168.29.1/Advanced_System_Content.asp](http://192.168.29.1/Advanced_System_Content.asp)
11. Open Putty and WinSCP and connect them to 192.168.29.1 using a admin:admin or admin:password *Tip: In WinSCP select SCP as file protocol, in Putty SSH*
12. In putty type: `cat /dev/mtd0 > original_cfe.bin`
13. In WinSCP refresh the window on the right side and note that original_cfe.bin is present
14. Copy original_cfe.bin to a local drive
15. Upload original_cfe.bin to [https://cfeditor.pipeline.sh/](https://cfeditor.pipeline.sh/) > Select 1.0.2.0 US AiMesh as Source CFE > Download the new .bin > rename it to new_cfe.bin
16. Upload **new_cfe.bin** & **mtd-write** & **FW_RT_AC68U_30043763626.trx** to router through WinSCP
17. In Putty type: `chmod u+x mtd-write`
18. In Putty type: `./mtd-write new_cfe.bin boot`
19. In Putty type: `mtd-write2 FW_RT_AC68U_30043763626.trx linux`
20. Perform NVRAM Reset, wait for reboot <5 mins *a. Power off router* *b. Wait 10 seconds* *c. Press and hold WPS button* *d. Power up the router and continue to hold WPS button for 15-20 seconds until power LED starts blinking very quickly.* 
21. Reset PC IP back to default
22. Log in to router using 192.168.1.1 and the router is now an AC68U with 64MB jffs username:/password is now: admin:admin
23. Enable SSH (see #10) and execute the code for fixing MTD5 partition that is listed below. Ezlink: [http://192.168.1.1/Advanced_System_Content.asp](http://192.168.1.1/Advanced_System_Content.asp)
24. You can now flash Asus, Merlin, Advanced Tomato, Tomato, and DD-WRT firmwares.

**Code for fixing MTD5 partition so you can update to latest firmware:** (this is entered in Putty after enabling SSH on the router)

`cat /dev/mtd5 > /jffs/mtd5_backup.bin mkdir /tmp/asus_jffs mount -t jffs2 /dev/mtdblock5 /tmp/asus_jffs rm -rf /tmp/asus_jffs/* sync && umount /tmp/asus_jffs rm -rf /jffs/.sys/RT-AC68U nvram unset fw_check && nvram commit && reboot`

**Video Example:**

- [*Windows*](https://www.youtube.com/watch?v=75ueHhUymCM)
- [*Mac*](https://www.youtube.com/watch?v=kBpugvj5kho)

**References Used:**

- Snbforums.com flash instructions: [flashInstructions_cDec04_16.pdf](https://www.snbforums.com/attachments/flashinstructions_cdec04_16-pdf.7919/)
- Bootymonger from slickdeals: [slickdeals.net](https://slickdeals.net/f/9330575-asus-tm-ac1900-wireless-ac1900-dual-band-gigabit-router-59-free-shipping?page=145#commentsBox)
- Wherever I found [https://cfeditor.pipeline.sh/](https://cfeditor.pipeline.sh/)
- [Comment from Albert Lee](https://www.bayareatechpros.com/ac1900-to-ac68u/#comment-653)

**Flashable Operating Systems:**

- [Merlin AC68U Builds](https://asuswrt.lostrealm.ca/download)
- [Official Builds](https://www.asus.com/us/Networking/RTAC68U/HelpDesk_Download/)
- [Advanced Tomato](https://advancedtomato.com/downloads/router/rt-ac68u)
- [Tomato by Shibby](https://advancedtomato.com/downloads/router/rt-ac68u)
- [DD-WRT](https://www.dd-wrt.com/wiki/index.php/Asus_RT-AC68U)

**Helpful information:**

- [Asus Rebadger](https://github.com/jasonbuechler/asus-rebadger) is a script that automates this process on a Windows 10 PC.
- If you have a fast connection you may benefit from Merlin or official builds as they have enabled hardware acceleration which allows the router to handle higher speeds.
- You may check router cpu speed at [http://192.168.1.1/Tools_Sysinfo.asp](http://192.168.1.1/Tools_Sysinfo.asp)
- If the 30/30/30 reset doesn’t work try this: 1. power off device 2. hold reset 20 seconds 3. power on device holding reset until power light blinks slowly
- If you can’t get Mini-CFE or Asus Restore utility to work try the method outlined in this comment: [https://www.bayareatechpros.com/ac1900-to-ac68u/#comment-5679](https://www.bayareatechpros.com/ac1900-to-ac68u/#comment-5679)
- There is discussion on the topic at [whirlpool forums](https://forums.whirlpool.net.au/thread/2693988).