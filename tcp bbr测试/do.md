
<font color = red size = 5>quic_test测试脚本</font>

---
## 过程：

1. 连接服务器端
2. 设置传输的文件 大小和丢包率，延迟，抖动等。
3. 得出传输后的时间数据
---

## 客户端相关操作：
echo "net.core.default_qdisc=fq" | sudo tee -a /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p


## 服务器端相关操作：
`加载内核模块`  
modprobe tcp_bbr  
echo "tcp_bbr" >> /etc/modules-load.d/modules.conf  
`修改内核参数：`  
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf  
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf  
sysctl -p


## 检测是否开启BBR:
lsmod | grep bbr
sysctl net.ipv4.tcp_available_congestion_control 
sysctl net.ipv4.tcp_congestion_control


----

## 设置网络情况：  

`测试文件大小200MB，可以用下面这个命令生成：`  
dd if=/dev/zero of=testFile bs=200M count=1 

`tc 命令来设置网络丢包、时延：`  
sudo tc qdisc add dev ens33 root netem limit 10000 loss 0% delay 0ms

`清除设置：`  
sudo tc qdisc del dev ens33 root

---

`查看网络状态：`  
tc qdisc


`查看当前的运行状态和速度：`  
sudo iftop


`抓包命令：`  
sudo tcpdump host ServerIp -w result.cap  
结束直接ctrl+c