

1. 开启usb共享网络

2. 手机 termux:ssh -f -N -D 0.0.0.0:11080 u0_a295@127.0.0.1 -p 8022

   PC:adb forward tcp:21080 tcp:11080

3. 通知栏termux开启"Aquire wakelock"

4. 手机联网，打开VPN；PC通过sosks代理至本机21080端口，注意规避127.0.0.1、localhost、192.168等

