# INSTALLATION OF PCAN DRIVER under ubuntu 20.04

```
sudo apt-get install libpopt-dev

wget https://www.peak-system.com/fileadmin/media/linux/files/peak-linux-driver-8.12.0.tar.gz
tar -xzf peak-linux-driver-8.12.0.tar.gz
cd peak-linux-driver-8.12.0/
make -C driver PCI=PCI_SUPPORT PCIEC=PCIEC_SUPPORT DNG=NO_DONGLE_SUPPORT USB=NO_USB_SUPPORT ISA=NO_ISA_SUPPORT PCC=NO_PCCARD_SUPPORT
sudo make -C driver install
make -C lib && sudo make -C lib install
make -C test && sudo make -C test install # test utilities (optional)
sudo modprobe pcan # driver loading
```

test : ```pcanview```


# SOME TESTS TO CONTACT MOTEUS CAN BOARD VIA A PEAK 4CAN M2 BOARD

change moteus controller ID :
-------------------------------

open tview

```python3.8 -m moteus_gui.tview --devices=11 --can-chan can0```


change id

``` conf write  ```

close

reopen tview

``` conf write  ```

# socketcan

open "can0" port with params :
------------------------------
```
sudo ip link set can0 up type can   tq 12 prop-seg 25 phase-seg1 25 phase-seg2 29 sjw 10   dtq 12 dprop-seg 6 dphase-seg1 2 dphase-seg2 7 dsjw 12   restart-ms 1000 fd on
```

open "can0" on Moteus Tview, and call QDD100 ID = 11 :
------------------------------------------------------
```python3.8 -m moteus_gui.tview --devices=11 --can-chan can0```


# pcan tests

modify params :
---------------
``` sudo echo 12 | sudo tee pcanpcifd0/data_sjw ```
for each of the 4 pcanpcifdX/  with each following param


nom_Tseq1 = 50


nom_Tseq2 = 29


nom_sjw=10


data_brp=1


data_tseg1=8


data_tseg2=7


data_sjw=12

overview of pcan interface :
----------------------------
```lspcan -T -t -i```

read pcan port properties :
---------------------------
```for f in /sys/class/pcan/pcanpcifd1/*; do [ -f $f ] && echo -n "`basename $f` = " && cat $f; done```

improving performance :
-----------------------
```sudo gedit /etc/modprobe.d/pcan.conf```

insert :
```
# pcan - automatic made entry, begin --------
# if required add options and remove comment
# options pcan type=isa,sp
options pcan fdirqtl=1
options pcan fdirqcl=1
options pcan fdusemsi=1
install pcan modprobe --ignore-install pcan
# pcan - automatic made entry, end ----------
```
exit !

reload :

```
sudo rmmod pcan & sudo modprobe pcan
```

# uninstall PCAN-M.2 Driver for Linux (Ubuntu 20.04)
```
cd ~/src/peak/peak-linux-driver-8.12.0/ 
sudo make uninstall
rmmod pcan
sudo reboot
```

