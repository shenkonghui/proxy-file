# other


## iptables透明代理
```
iptables -t nat -N shadowsocks
# 保留地址、私有地址、回环地址 不走代理
iptables -t nat -A shadowsocks -d 0/8 -j RETURN
iptables -t nat -A shadowsocks -d 127/8 -j RETURN
# iptables -t nat -A shadowsocks -d 10/8 -j RETURN
iptables -t nat -A shadowsocks -d 169.254/16 -j RETURN
iptables -t nat -A shadowsocks -d 172.16/12 -j RETURN
iptables -t nat -A shadowsocks -d 192.168/16 -j RETURN
iptables -t nat -A shadowsocks -d 224/4 -j RETURN
iptables -t nat -A shadowsocks -d 240/4 -j RETURN
# 以下IP为局域网内不走代理的设备IP
# iptables -t nat -A shadowsocks -s 192.168.2.10 -j RETURN
# 发往shadowsocks服务器的数据不走代理，否则陷入死循环
# 替换111.111.111.111为你的ss服务器ip/域名
iptables -t nat -A shadowsocks -d  117.187.32.193 -j RETURN    
# 大陆地址不走代理，因为这毫无意义，绕一大圈很费劲的
iptables -t nat -A shadowsocks -m set --match-set cidr_cn dst -j RETURN
# 其余的全部重定向至ss-redir监听端口1080(端口号随意,统一就行)
iptables -t nat -A shadowsocks ! -p icmp -j REDIRECT --to-ports 1081
# OUTPUT链添加一条规则，重定向至shadowsocks链
iptables -t nat -A OUTPUT ! -p icmp -j shadowsocks
iptables -t nat -A PREROUTING ! -p icmp -j shadowsocks
```
