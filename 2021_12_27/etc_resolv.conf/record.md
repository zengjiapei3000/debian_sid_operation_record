# /etc/resolv.conf

2021/12/28 更新: 最终手动管理 `/etc/resolv.conf`.

---

太多的程序抢占然后修改 `/etc/resolv.conf`的内容, 如 dhcp, systemd-resolvd, openresolvf(和 systemd-resolvd 冲突), 以及各种 vpn.

发现 `$ ping baidu.com` 报错: `ping error: Name or service not known`, 
但是 ping 百度的ip却能成功. 一查看 `/etc/resolv.conf`, 发现内容:
```
# Generated by resolvconf
domain DHCP
search DHCP HOST
nameserver 127.0.0.1
```
可能由于安装 `isc-dhcp-server` 包.
改成
```
nameserver 192.168.0.1
```
能 ping baidu.com 了.

(待补充)

## openresolv
考虑用 `openresolv` 管理这一系列文件, **好处**是能分开管理各个程序的文件.(放弃于2021/12/28)
**重要**: `openresolv` 与 `resolvconf` 冲突！！

**tips**: 当更新过`/etc/resolvconf.conf`后, 要运行`resolvconf -u`来应用新设置.

**手册页面**: 
- openresolv官网: https://roy.marples.name/projects/openresolv/
- resolvconf命令(root下使用):  https://manpages.debian.org/unstable/openresolv/resolvconf.8.en.html
- resolvctl命令(可选, 不推荐, user下使用):  https://manpages.debian.org/unstable/systemd/resolvconf.1.en.html
```
# Configuration for resolvconf(8)
# See resolvconf.conf(5) for details

resolv_conf=/etc/resolv.conf
# If you run a local name server, you should uncomment the below line and
# configure your subscribers configuration files below.
#name_servers=127.0.0.1


# Mirror the Debian package defaults for the below resolvers
# so that resolvconf integrates seemlessly.
dnsmasq_resolv=/var/run/dnsmasq/resolv.conf
pdnsd_conf=/etc/pdnsd.conf
unbound_conf=/etc/unbound/unbound.conf.d/resolvconf_resolvers.conf
```

**参考**:
1. https://cloud.tencent.com/developer/article/1710514