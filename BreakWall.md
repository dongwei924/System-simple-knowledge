# Server
1. AWS AWS-Key
saved in dongwei924@163.com
pwd _+e+***+_

2. ssh 登录 aws
  cd <save pem file path>
  ssh -i "<your pem file>" ubuntu@<your Public DNS (IPv4)>

3. 修改root密码（optional）
  sudo passwd root
  su root
 
4. 安装shadowsocks
  pip install shadowsocks (sudo apt-get install shadowsocks)
  
5. 配置shadowsocks-json
vim /etc/shadowsocks.json
{
  "server": "0.0.0.0",
  "server_port": 8080,
  "local_address": "127.0.0.1",
  "local_port": 1080,
  "password": "your passwd",
  "timeout": 300,
  "method": "aes-256-cfb",
}

6. 启动ss服务
ssserver -c /etc/shadowsocks.json -d start

7. 添加group
 1. 在控制管理后台里面，点击Security Groups选项，选择自己服务器对应的group ID(Name)
 2. 进去之后，选择下方的Inbound选项，点击Edit按钮
 3. 新增Custom TCP Rule，Port Range里面填的就是第3步里面json文件对应的server_port:8080
 
# Client Config：
1. 安装shadowsocks：
sudo pip install shadowsocks（sudo apt-get install shadowsocks）

2. 配置shadowsocks
sudo gedit /etc/shadowsocks.json
{
    "server":"my_server_ip",
    "server_port":8388,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"mypassword",
    "timeout":520,
    "method":"aes-256-cfb",
}
#{
#    "server":"18.220.28.197",
#    "server_port": 8081,
#    "local_address": "127.0.0.1",
#    "local_port": 1081,
#    "password":"123456",
#    "timeout":300,
#    "method":"aes-256-cfb"
#} 

3. 启动服务
sslocal -c /etc/shadowsocks.json

4. 系统配置
Settings-> Network -> Network Proxy 
change to manual, Sock Host:127.0.0.1 Port:1081


