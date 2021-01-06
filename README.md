![Linux bandeau](images/bande_linux.jpg)

# Linux_cmd_informations

Information on Linux command

# File actions
```
ls -als           // list all files
mkdir titi        // make directory "titi"
rmdir -rf  titi   // remove all file on the "titi" directory
cd                // Go to your home directory
cd                // Go to the upper directory
```

# update linux with root profile
```
sudo apt-get update       //update all the package on your distribution
sudo atp-get upgrade      //upgrade all package installed on your distribution
sudo apt-get install gimp //exemple to install gimp software for example you can replace gimp by other software....
sudo apt-get remove gimp  //remove gimp software for example. You can replace gimp by other software for removing....
```

# network information
```
iwconfig //see status of wireless card.
ifconfig //see adress ip of each network card.

sudo iwlist scan  //scan les connexion wifi

//arret demmarage carte wifi
sudo ifconfig wlp5s0 up    //connexion de la carte wlp5s0
sudo ifconfig wlp5s0 down  //deconnexion de la carte wlp5s0
```

# network configuration example
```
sudo nano /etc/network/interfaces //edit the network conf file.
config example
STATIC
 iface wlp5s0 inet static
 address 192.168.1.10
 netmask 255.255.255.0
 gateway 192.168.1.254
 wpa-psk "replace by your wpa password securit"
 wpa-driver wext
 wpa-key-mgmt WPA-PSK
 wpa-proto WPA2
 wpa-ssid "replace by your SSID"
 auto wlp5s0

DYNAMIC
 iface wlp5s0 inet dhcp
 auto wlp5s0

After saving file run these commands :
 sudo service networking force-reload
 sudo service networking restart
 sudo iwconfig  //for controlling if your connexion was OK
```
# dns configuration
```
sudo nano /etc/resolv.conf
add dns as well :
 nameserver 212.27.40.240
```

# Main Issues for my specifics problems
## ISSUE 1 disconnect wifi card intel PRO wireless 3945ABG on LUBUNTU
```
sudo rmmod iwl3945     //Software disconnect the wifi board 
sudo modprobe iwl3945  //Software reconnect the wifi board
```

## ISSUE 2 connect on windows remote desktop
```
install rdesktop
  sudo apt-get install rdesktop
Run rdesktop with rdp to IP.IP.IP.IP adress with screen size 1440x900 with 16bits color and sound stay on the distant computer.
  rdesktop -r sound:remote -k fr -g 1440x900 -a 16 IP.IP.IP.IP
  rdesktop -k fr -a 8 IP.IP.IP.IP  //color 8bit full screen keyboard FR
```

## ISSUE 3 ssh connexion
```
ssh user@IP.IP.IP.IP //run ssh connexion with user "user" at adress IP.IP.IP.IP
ssh -X user@IP.IP.IP.IP //run ssh with server X. 
```
## ISSUE 4 test network bandwith
```
nc -lk 2112 >/dev/null  //run on the remote computer 10.10.10.1
dd if=/dev/zero bs=16000 count=625 | nc -v 10.10.10.1 2112  //transfert 10MB to the remote computer and evaluate the bandwith.

Response example on the local computer :
  Connection to 10.10.10.1 2112 port [tcp/*] succeeded!
  625+0 enregistrements lus
  625+0 enregistrements écrits
  10000000 bytes (10 MB, 9,5 MiB) copied, 4,08256 s, 2,4 MB/s
```
## ISSUE 5 : Hard drive informations
```
sudo blkid  //To know informations concerning hard drive.

sudo hdparm -C  /dev/sda      //See the hard drive status
sudo hdparm -y  /dev/sda      //Put the hard drive sda in standby mode

sudo hdparm -S 1 /dev/sda     //le chiffre 1 correspond au temps par multiples de 5 secondes. 
sudo hdparm -S 120 /dev/sda1  //put the disk in standby mode after 120*5s=10 minutes

sudo hdparm -I /dev/sda | grep level    //To verify if the hard drive support low power management. If the disk support low power you will see 254 by default.
```

## ISSUE 6 : Delete shell history
```
Delete specific line
 history -d 505   // delete ligne 505 in the istory command
or edit the local bash history 
 nano ~/.bash_history 
```
## ISSUE 7 : Audio configuration with USB-AUDIO
```
List of USB device :
lsusb

List aof audio device :
aplay -l

Edit  /etc/asound.conf
Modify as well :
  pcm.!default {
          type hw
          card 0
          device 0
  }

  ctl.!default {
          type hw
          card 0
          device 0
  }

Nota : if the local file ~/.asoundrc exist, it have more priority. 
```

## ISSUE 8 : Change the icon of the login on Lubuntu under XFCE
```
Place an icon in PNG format on the home directory of the user.
Rename it to .face
That's all.
```

## ISSUE 9 : Change Welcome message Shell linux

Edit the file /etc/motd
```
sudo nano /etc/motd
```

You can use this link to form special text message :http://www.patorjk.com/software/taag

After modify your motd (Message Of The Day) file when you log in ssh with your terminal, the new text message appears.

## ISSUE 10 : Find big file on the system

You can use this command for file up to 500Mo

```
sudo find /PATH/ -type f -size +500000k -exec ls -lh {} \; | awk '{ print $9 ": " $5 }'
```

## ISSUE 11 : How To Create a New User and Grant Permissions in MySQL

Run MYSQL : ** sudo mysql **

```
// add user
mysql> CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
// add grand permission
mysql> GRANT ALL PRIVILEGES ON * . * TO 'newuser'@'localhost';
// relaod privilege
mysql> FLUSH PRIVILEGES;
```
link : https://www.digitalocean.com/community/tutorials/how-to-create-a-new-user-and-grant-permissions-in-mysql

## ISSUE 12 : Intempestive wifi disconnexion

For resolve this problem, it is necessary to disconnect the power save of the wifi connexion.

It is important to do this manipulation for each wifi network.

```
cd /etc/NetworkManager/system-connections/
ls
sudo nano your_wifi_network

Add "powersave=2" on the wifi section, save the file and reboot the system

```
To see if the configuration was ok run : **iwconfig** and watch if you see ***"Power Management:off"***

# linux_informations
List of linux commands use the wiki for informations

## list of the 10 lines at the end the auth.log file
tail -n 10 /var/log/auth.log

If auth.log is empty, you can restart the log service.
```
sudo /etc/init.d/rsyslog restart
```

## List end line of open session
last -n 1

# Serveur Email at home
List of command for installing email server at home.

## Install the package bind9-host
sudo apt-get install bind9-host

for testing
host -t MX <IP or domain>
 
```
Example :
host -t MX google.com
 google.com mail is handled by 10 aspmx.l.google.com.
 google.com mail is handled by 40 alt3.aspmx.l.google.com.
 google.com mail is handled by 50 alt4.aspmx.l.google.com.
 google.com mail is handled by 30 alt2.aspmx.l.google.com.
 google.com mail is handled by 20 alt1.aspmx.l.google.com.

```

## Empty file

For erasing the content of a file, you can send this command :

```
echo > filename.txt
```

with this command it is not necessary to delete file and recreate it.

## How to know the directory size

```
du -h   // for list size directory
du -sh /etc   //total size of etc directroy
```
