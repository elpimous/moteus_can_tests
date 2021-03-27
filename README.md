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
