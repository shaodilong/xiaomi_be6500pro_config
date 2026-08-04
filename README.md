小米be6500pro路由器，smartdns分流+adguardhome过滤广告+shellcrash科学上网自用配置

shellcrash安装：
export url='https://fastly.jsdelivr.net/gh/juewuy/ShellCrash@master' && sh -c "$(curl -kfsSl $url/install.sh)" && source /etc/profile &> /dev/null
在/data/other_vol/目录下安装，配置好后将dns服务器修改为127.0.0.1:5553(AdGuardHome)

smartdns安装(建议将配置文件的国内dns改成自己地区的dns):
将smartdns,smartdns.conf和china-list通过winscp放到/data/other_vol/
通过命令运行
/data/other_vol/smartdns -c /data/other_vol/smartdns.conf
停止运行
killall smartdns

AdGuardHome安装:
将AdGuardHome，AdGuardHome.yaml通过winscp放到/data/other_vol/
AdGuardHome 常用命令：
启动服务：/data/other_vol/AdGuardHome -s start
停止服务：/data/other_vol/AdGuardHome -s stop
重启服务：/data/other_vol/AdGuardHome -s restart
后台管理端口3000,监听端口5553
上游服务器设置127.0.0.1:5335(SmartDNS)

运行好后要通过iptables将53端口指向smartdns(5335)
iptables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-ports 5335
iptables -t nat -A PREROUTING -p tcp --dport 53 -j REDIRECT --to-ports 5335
ip6tables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-ports 5335
ip6tables -t nat -A PREROUTING -p tcp --dport 53 -j REDIRECT --to-ports 5335

后续更新
shellcrash可以通过ssh输入crash更新
AdguardHome要在官方github:https://github.com/AdguardTeam/AdGuardHome/releases下载AdGuardHome_linux_amd64.tar.gz
下载后解压通过upx压缩https://github.com/upx/upx/releases
smartdns在官方github:https://github.com/pymumu/smartdns/releases下载smartdns-aarch64
也是通过upx压缩再上传到/data/other_vol/
china-list使用v2ray-rules-dat规则集:https://github.com/Loyalsoldier/v2ray-rules-dat/releases

PS:如果还有问题就问AI吧




