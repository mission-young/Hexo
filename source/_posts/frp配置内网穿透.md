title: frp配置内网穿透
author: 远方
tags:
  - 网络
  - 内网穿透
  - frp
categories:
  - 技术
date: 2019-08-05 19:20:00
---
## frp的作用

- 利用处于内网或防火墙后的机器，对外网环境提供 http 或 https 服务。
- 对于 http, https 服务支持基于域名的虚拟主机，支持自定义域名绑定，使多个域名可以共用一个80端口。
- 利用处于内网或防火墙后的机器，对外网环境提供 tcp 和 udp 服务，例如在家里通过 ssh 访问处于公司内网环境内的主机。

## frp服务器端配置(公网ip)
- user: server
- passwd: serverpasswd 
- ip: 162.105.2.3
- host: server.com
```
[common]
bind_port = 7000
vhost_http_port = 8000

[web03]
listen_port = 5000

[web04]
listen_port = 5001
```
### frp客户机端配置(私网ip)
- user: client
- passwd: clientpasswd
- 
```
[common]
server_addr = service.com
server_port = 7000

[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 7001

[web01]
type = http
local_port = 4000
custom_domains = blog.service.com

[web02]
type = http
local_port = 8888
custom_domains = nb.service.com

[web03]
type = tcp
local_ip = 127.0.0.1
local_port = 8889
remote_port = 5000

[web04]
type = tcp
local_ip = 127.0.0.1
local_port = 8890
remote_port = 5001
```

## ssh代理服务
```bash
ssh -oPort=7001 -Y client@162.105.2.3
## passwd:clientpasswd
```
## web代理服务
- web01 http://blog.service.com:8000
- web02 http://nb.service.com:8000
- web03 http://162.105.2.3:5000 http://service.com:5000
- web04 http://162.105.2.3:5001 http://service.com:5001
1. 当公网ip有域名时,采用http和tcp的方式都可以访问内网服务。
2. 采用http协议时，需设置子域名，此时共用一个vhost_http端口。
3. 采用tcp协议时，需对每一个内网服务，单独开一个端口。这些端口在frpc.ini中设置，每个内网端口对应一个外网端口。
4. 只有公网ip而没有域名时，且需设置多个内网服务，可以采用tcp的方式。