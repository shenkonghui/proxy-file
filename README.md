# other


## iptables透明代理
#!/bin/bash 
iptables -F
# Create new chain 
iptables -t nat -N SHADOWSOCKS  
iptables -t nat -A PREROUTING -p tcp -j SHADOWSOCKS  
# Ignore your shadowsocks server’s addresses 
# It’s very IMPORTANT, just be careful. 
iptables -t nat -A SHADOWSOCKS -d 服务器ip -j RETURN  
# 这一步非常重要！到服务端的出口数据不要再进行重定向，否则进入死循环 
#去除内网地址 
iptables -t nat -A SHADOWSOCKS -d 0.0.0.0 -j RETURN  
iptables -t nat -A SHADOWSOCKS -d 127.0.0.1 -j RETURN  
iptables -t nat -A SHADOWSOCKS -d 192.168.0.0/16 -j RETURN

iptables -t nat -A SHADOWSOCKS -d 8.8.0.0/16 -j RETURN  
iptables -t nat -A SHADOWSOCKS -d 0.0.0.0/8 -j RETURN  
#iptables -t nat -A SHADOWSOCKS -d 10.0.0.0/8 -j RETURN  
iptables -t nat -A SHADOWSOCKS -d 127.0.0.0/8 -j RETURN  
iptables -t nat -A SHADOWSOCKS -d 169.254.0.0/16 -j RETURN  
iptables -t nat -A SHADOWSOCKS -d 172.16.0.0/12 -j RETURN  
iptables -t nat -A SHADOWSOCKS -d 192.168.0.0/16 -j RETURN  
iptables -t nat -A SHADOWSOCKS -d 224.0.0.0/4 -j RETURN  
iptables -t nat -A SHADOWSOCKS -d 240.0.0.0/4 -j RETURN  
iptables -t nat -A SHADOWSOCKS -d 0.0.0.0/254.0.0.0 -j RETURN

#还可以加如国内ip段参见 https://gist.github.com/wen-long/8644507 
# Anything else should be redirected to shadowsocks’s local port 
iptables -t nat -A SHADOWSOCKS -p tcp -–to-ports 1081 -j REDIRECT 
