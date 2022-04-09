# kobo-libra2-uhid-module

My Logitech R500 can't work on Kobo Libra 2 so I try to check the problem. Finally, I rebuild the uhid.ko and it works with R500.

# How to use it

1. Install NickelMenu to launch it.
2. Copy "kobo-libra2-uhid-module" foler to ".adds".
3. Copy "kobo-libra2-uhid-module/config" to ".adds/nm" or you could add the text below to your own config if you already use NickelMenu.
```
menu_item :main    :Bluetooth Patch (toggle) :cmd_output         :500:quiet :rmmod -w /mnt/onboard/.adds/kobo-libra2-uhid-module/uhid.ko
  chain_success:skip:3
  chain_failure                              :cmd_spawn          :quiet :exec /mnt/onboard/.adds/kobo-libra2-uhid-module/run.sh
  chain_success                              :dbg_toast          :insmod uhid.ko successfully
  chain_always:skip:-1
  chain_success                              :dbg_toast          :rmod -w uhid.ko successfully
```