1-一键安装脚本复制到服务器，选择4（目的是安装协议lis那个）
wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/ssr.sh && chmod +x ssr.sh && bash ssr.sh

2-安装好之后，禁用ipv6
cat >>  /etc/sysctl.conf << EOF
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
EOF
以上这个代码块全部复制进去

开启BBR加速

挨个复制 echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
挨个复制 echo "net.ipv4.tcp_congestion_control=bbr" >>/etc/sysctl.conf
挨个复制 sysctl -p


安装docker
$ apt-get install curl
$ docker version > /dev/null || curl -fsSL get.docker.com | bash
$ service docker restart


docker对接
$ docker run -d --name=ssrmu -e NODE_ID=你的节点ID -e API_INTERFACE=modwebapi -e WEBAPI_URL="https://your_domain" -e WEBAPI_TOKEN=your_webapi -e MU_SUFFIX=microsoft.com -e SPEEDTEST=0 --network=host --log-opt max-size=50m --log-opt max-file=3 --restart=always fanvinga/docker-ssrmu
