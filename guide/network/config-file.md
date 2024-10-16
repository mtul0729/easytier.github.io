# 配置文件

支持使用 -c 参数指定配置文件路径。

注意：配置文件的优先级更高，当运行时指定了配置文件，则命令行中除 -c 的其他参数将被忽略，只对配置文件生效。

```sh
./easytier-core -c ./config.toml
```

在不使用参数的情况下直接运行 `./easytier-core` 可以获得最小配置文件。使用参数运行可以获得对应参数的配置文件。配置文件会打印在命令行中，可以手动复制对应配置保存为toml文件即可。

下面是一个配置文件的示例以及各种配置项的注解。

```toml
# 实例名称，用于在同一台机器上标识此 VPN 节点
instance_name = ""
# 主机名，用于标识此设备的主机名
hostname = ""
# 实例 ID，一般为 UUID，在同一个 VPN 网络中唯一
instance_id = ""
# 此 VPN 节点的 IPv4 地址，如果为空，则此节点将仅转发数据包，不会创建 TUN 设备
ipv4 = ""
# 由 Easytier 自动确定并设置IP地址，默认从10.0.0.1开始。警告：在使用 DHCP 时，如果网络中出现 IP 冲突，IP 将自动更改
dhcp = false

# 监听器列表，用于接受连接
listeners = [
"tcp://0.0.0.0:11010",
"udp://0.0.0.0:11010",
"wg://0.0.0.0:11011",
"ws://0.0.0.0:11011/",
"wss://0.0.0.0:11012/",
]

# 退出节点列表
exit_nodes = [
]

# 用于管理的 RPC 门户地址
rpc_portal = "127.0.0.1:15888"

[network_identity]
# 网络名称，用于标识 VPN 网络
network_name = ""
# 网络密钥，用于验证此节点属于 VPN 网络
network_secret = ""

# 这里是对等连接节点配置，可以多段配置
[[peer]]
uri = ""

[[peer]]
uri = ""

# 这里是子网代理节点配置，可以有多段配置
[[proxy_network]]
cidr = "10.0.1.0/24"

[[proxy_network]]
cidr = "10.0.2.0/24"

#wg配置信息
[vpn_portal_config]
#VPN客户端所在的网段，下面为示例
client_cidr = "10.14.14.0/24"
#wg所监听的端口(请勿和listeners的wg冲突)
wireguard_listen = "0.0.0.0:11012"

[flags]
# 连接到对等节点使用的默认协议
default_protocol = "tcp"
# TUN 设备名称，如果为空，则使用默认名称
dev_name = ""
# 是否启用加密
enable_encryption = true
# 是否启用 IPv6 支持
enable_ipv6 = true
# TUN 设备的 MTU
mtu = 1380
# 延迟优先模式，将尝试使用最低延迟路径转发流量，默认使用最短路径
latency_first = false
# 将本节点配置为退出节点
enable_exit_node = false
# 禁用 TUN 设备
no_tun = false
# 为子网代理启用 smoltcp 堆栈
use_smoltcp = false
# 仅转发白名单网络的流量，支持通配符字符串。多个网络名称间可以使用英文空格间隔。如果该参数为空，则禁用转发。默认允许所有网络。例如：'*'（所有网络），'def*'（以def为前缀的网络），'net1 net2'（只允许net1和net2）
foreign_network_whitelist = "*"
```
