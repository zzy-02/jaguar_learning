### ip简介：
ip命令是 iproute2 软件包中的一个强大工具，用于执行网络管理相关任务。
它是早期网络工具（如 ifconfig、route 和 arp）的现代替代方案。
ip 命令 可以用于查看或配置路由、网络设备、网络接口以及隧道。
语法通常为：

```
ip [ OPTIONS ] OBJECT { COMMAND | help }
```
IP 用来“找位置”，MAC 用来“找具体那台设备”
计算机发包的流程：
1. 先查路由表(route)，找到目标 IP 属于哪个网段(IP层)
2. 再查 ARP 表，找到目标 IP 对应的 MAC 地址(IP -> MAC)
3. 最后通过 MAC 地址找到具体的设备(数据链路层)

route 决定方向
网关是中转
ARP 负责翻译
MAC 才真正把包送走



### ip命令的主要功能包括：

接口管理（Interface Management）：配置和监控网络接口

IP 地址管理（IP Address Management）：添加、删除和显示 IP 地址

路由控制（Routing Control）：管理路由表和路由规则
- 主要用于配置静态路由，设置默认网关等，处于网络层

邻居管理（Neighbor Management）：处理 ARP / 邻居缓存条目
- 主要用于解析 IP 地址到 MAC 地址的映射，处于数据链路层

网络命名空间（Network Namespaces）：操作和管理网络命名空间
- 主要用于隔离网络环境，实现虚拟化网络，处于操作系统层

隧道（Tunneling）：创建和管理网络隧道
- 主要用于封装和传输数据包，支持 VPN 等应用，处于网络层

### 常用ip命令示例：
1. 基本使用
```
ip link show               # 显示所有网络接口
ip link show eth0          # 显示特定接口 eth0 的信息
ip -s link show eth0       # 显示 eth0 接口的统计信息

ip addr show               # 显示所有接口的 IP 地址
ip addr show eth0          # 显示 eth0 接口的 IP 地址
ip addr add 192.168.1.100/24 dev eth0  # 为 eth0 接口添加 IP 地址
ip addr del 192.168.1.100/24 dev eth0  # 从 eth0 接口删除 IP 地址
ip addr flush dev eth0      # 清除 eth0 接口的所有 IP 地址
```
2. 接口管理
```
ip link set eth0 up        # 启用 eth0 接口
ip link set eth0 down      # 禁用 eth0 接口
ip link set eth0 mtu 1400  # 设置 eth0 接口的 MTU 大小为 1400
# MTU（Maximum Transmission Unit）是网络接口能够传输的最大数据包大小
ip link set eth0 addr 00:11:22:33:44:55  # 设置 eth0 接口的 MAC 地址

# 创建虚拟接口(不太懂)
ip link add link eth0 name eth0.100 type vlan id 100  # 创建 VLAN 接口 eth0.100
ip link add name br0 type bridge                  # 创建桥接接口 br0
ip link add veth0 type veth peer name veth1          # 创建一对虚拟以太网接口 veth0 和 veth1
ip link delete veth0                             # 删除虚拟以太网接口 veth0
```
ps：创建虚拟接口的目的是为了实现网络隔离、虚拟化和灵活的网络配置。

3. 路由管理
```
ip route show               # 显示路由表
ip route get 8.8.8.8          # 获取到达目标 IP 的路由信息
ip route show dev eth0       # 显示通过 eth0 接口的路由

ip route add default via 192.168.1.1  # 添加默认路由，网关为 192.168.1.1
ip route add 10.0.0.0/8 via 192.168.1.1  # 添加到 10.0.0.0/8 网段的路由，网关为 192.168.1.1
ip route del 10.0.0.0/8          # 删除到 10.0.0.0/8 网段的路由
ip route replace default via 192.168.1.254  # 替换默认路由，网关为 192.168.1.254

# 多路由表
ip route show table  # 显示所有路由表
ip route add 192.168.2.0/24 via 10.0.0.1 table 100  # 向路由表 100 添加路由
ip route show table 100  # 显示路由表 100
ip rule add from 192.168.1.0/24 table 100  # 添加规则，将来自 192.168.1.0/24 网段的流量使用路由表 100
```

4. ARP管理
```
ip neighbor show               # 显示邻居缓存（ARP 表）
ip neigh add 192.168.1.50 lladdr 00:11:22:33:44:55 dev eth0 # 添加静态 ARP 条目,将 IP 地址映射到 MAC 地址
ip neigh del 192.168.1.50 dev eth0  # 删除静态 ARP 条目
ip neigh flush dev eth0       # 清除 eth0 接口的所有邻居缓存条目

```

5. Network Namespaces
```
ip netns list                 # 列出所有网络命名空间
ip netns add ns1              # 创建名为 ns1 的网络命名空间
ip link set eth0 netns ns1     # 将 eth0 接口移动到 ns1 命名空间
ip nets exec ns1 ip addr show  # 在 ns1 命名空间中显示 IP 地址
ip netns exec ns1 ip link set eth0 netns 1      # 将 eth0 接口从 ns1 命名空间移回默认命名空间
ip netns delete ns1           # 删除名为 ns1 的网络命名空间

注意：使用结束要退出命名空间，否则会影响后续操作，可以使用 `exit` 命令退出当前命名空间。
```

6. 隧道管理
隧道用于在两个网络节点之间封装和传输数据包，常见的隧道类型包括 GRE（Generic Routing Encapsulation）和 IPIP（IP-in-IP）。
```
ip tunnel add gre1 mode gre remote 10.0.0.2 local 10.0.0.1 ttl 255  # 创建 GRE 隧道 gre1,远程端为 10.0.0.2，本地端为 10.0.0.1
ip tunnel add ipip1 mode ipip remote 192.168.1.2 local 192.168.1.1 ttl 255  # 创建 IPIP 隧道 ipip1,远程端为 192.168.1.2，本地端为 192.168.1.1
ip tunnel show                # 显示所有隧道接口
ip tunnel del gre1            # 删除 GRE 隧道 gre1
```


7. Monitoring and Statistics 监控与统计
```

```


devlink dev show


