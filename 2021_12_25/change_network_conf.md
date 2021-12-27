# shell history
```
$ sudo cp /etc/network/interfaces /etc/network/interfaces.bak 
$ sudo vim /etc/network/interfaces 
$ sudo /etc/init.d/networking restart
```
后来发现 网络已经不是init.d 管理了, 应该由 systemd 管理.

2021/12/28 更新:
`$ sudo cp /etc/network/interfaces /etc/network/interfaces.bak`
已还原:
`$ sudo cp /etc/network/interfaces.bak /etc/network/interfaces`