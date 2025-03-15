# Ubuntu 24.04.2 LTS
Most of the installation went smoothly for the beelinks
(the reason everything is bee themes is due to the cluster being on Beelink EQ mini PCs)

#### quick install link 
https://ubuntu.com/download/server

## Device names
* honey1
* honey2
* honey3

### Snaps

* ectd
* poweshell - why not

### Problem 1

#### honey3 - Prev OS clash 
previously the OS for honey3 was a proxmox metal install the other honeys where a talos install ill get into why this is important in a minute
for whatever reason whenever I would mount the storage on honey3 the thing would crash, after reading the report i narrowed it down to ethier the UEFI settings in the BIOS because on this machine i was messing around in it on project
Or it was unable write over the previous mounted drive

##### Actions I took
* First thing i did was to reset the UEFI settings to the default for the machine and made sure secure boot was disabled 
    * this did *not* work
* Second I installed talos on the machine to make the variables more constant
* Third I changed for UEFI to Legacy just for the next installation 

It worked and fixed itself I cant tell you exactly worked but this did the trick

### Disable the Firewall 

for some reason ubuntu comes with a preinstalled and activated firewall
this will get in the way later so we need to remove that 

first i SSH'd into the machines

then on all 3 i did 
`sudo ufw disable` - temp disable
&
`sudo systemctl disable ufw` - disables it at boot















