安装jumpserver

关闭防火墙

 systemctl stop firewalld
 setenforce 0



自动化部署

curl -sSL https://github.com/jumpserver/jumpserver/releases/download/v2.8.2/quick_start.sh | bash



安装目录 /opt/jumpserver-install-v2.8.2

cd /opt/jumpserver-installer-v2.8.2



Install

./jmsctl.sh install



Help

./jmsctl.sh -h



Upgrade

./jmsctl.sh check_update



