Title         : V2ray
Author        : You
Logo          : True

[TITLE]

系统要求

CentOS 7+、Debian 8+bunts 16+，作者更推荐Debian 9.

安装curl 

CentOS 命令： yum update -y && yum install curl -y

Debian/Ubuntu 命令：apt-get update -y && apt-get install curl -y

安装脚本

bash <(curl -s -L https://git.io/v2ray.sh)

下面会让你选择V2ray的传输协议，可选协议众多，如果没什么特殊需求的话，默认TCP即可。如果你需要启用类似KCPTUN的网络加速功能，则选择mKCP开头的几个，在mKCP协议下，流量以UDP的形式传输。

启动命令 v2ray

