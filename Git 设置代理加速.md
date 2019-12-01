- 设置代理


> git config --global http.proxy 'socks5://127.0.0.1:1080'
> git config --global https.proxy 'socks5://127.0.0.1:1080'
>
> 端口号1080为VPN设置的代理端口，可自行修改为自己的VPN代理端口，楼主使用第一种无加速效果，第二种代理方式速度得到明显提升，峰值可达1M/s。

- 取消代理

> git config --global --unset http.proxy
> git config --global --unset https.proxy