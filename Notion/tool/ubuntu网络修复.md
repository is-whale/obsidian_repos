`sudo lshw -c Network`

可见 Ethernet interface 状态为 `DISABLE` ，听说这个是[Ubuntu Network Manager Bug](https://link.zhihu.com/?target=https%3A//bugs.launchpad.net/ubuntu/%2Bsource/network-manager/%2Bbug/1638842)，经过网上查询得知可以创建一个缺少的配置文件可以解决这个问题，

`sudo touch /etc/NetworkManager/conf.d/10-globally-managed-devices.conf sudo systemctl restart NetworkManager`