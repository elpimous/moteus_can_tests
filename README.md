## some tests to talk to moteus board with Peak 4 can M2 board

# SOCKETCAN

open "can0" port with params :
-----------------------
'''
sudo ip link set can0 up type can   tq 12 prop-seg 25 phase-seg1 25 phase-seg2 29 sjw 10   dtq 12 dprop-seg 6 dphase-seg1 2 dphase-seg2 7 dsjw 12   restart-ms 1000 fd on
'''

open "can0" on Moteus Tview, and call QDD100 ID = 11 :
------------------------------------------------------
python3.8 -m moteus_gui.tview --devices=11 --can-chan can0
