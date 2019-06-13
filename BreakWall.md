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
//#{
//#    "server":"18.220.28.197",
//#    "server_port": 8081,
//#    "local_address": "127.0.0.1",
//#    "local_port": 1081,
//#    "password":"123456",
//#    "timeout":300,
//#    "method":"aes-256-cfb"
//#} 

3. 启动服务
sslocal -c /etc/shadowsocks.json

4. 系统配置
Settings-> Network -> Network Proxy 
change to manual, Sock Host:127.0.0.1 Port:1081


5. polipo
However, some command line tools (such as npm) don’t support socks5 protocol, and under the help of polipo we can convert socks5 into http.

For Ubuntu:

sudo apt-get install polipo
Edit the config file at /etc/polipo/config, and append the following two lines:

socksParentProxy = "localhost:1080"
socksProxyType = socks5
Finally, restart polipo:

sudo service polipo stop
sudo service polipo start
And for Mac:

brew install polipo
Edit file /usr/local/opt/polipo/homebrew.mxcl.polipo.plist, add the socksParentProxy:

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
  <dict>
    <key>Label</key>
    <string>homebrew.mxcl.polipo</string>
    <key>RunAtLoad</key>
    <true/>
    <key>KeepAlive</key>
    <true/>
    <key>ProgramArguments</key>
    <array>
      <string>/usr/local/opt/polipo/bin/polipo</string>
      <string>socksParentProxy=localhost:1080</string>
    </array>
    <!-- Set `ulimit -n 65536`. The default macOS limit is 256, that's
         not enough for Polipo (displays 'too many files open' errors).
         It seems like you have no reason to lower this limit
         (and unlikely will want to raise it). -->
    <key>SoftResourceLimits</key>
    <dict>
      <key>NumberOfFiles</key>
      <integer>65536</integer>
    </dict>
  </dict>
</plist>
Restart polipo:

launchctl unload ~/Library/LaunchAgents/homebrew.mxcl.polipo.plist
launchctl load ~/Library/LaunchAgents/homebrew.mxcl.polipo.plis
Also you can make it start when the system launches:

ln -sfv /usr/local/opt/polipo/*.plist ~/Library/LaunchAgents
Now you can the the http proxy offered by polipo:

$ http_proxy=http://localhost:8123 curl ip.gs
当前 IP：139.162.10.88 来自：新加坡新加坡 linode.com
You can set it globally by (here I just add https_proxy by the way):

export http_proxy=http://localhost:8123
export https_proxy=http://localhost:8123
And stop the proxy with unset http_proxy or (unset https_proxy).


Create /etc/polipo.conf
daemonise = false
pidFile = /tmp/polipo.pid
proxyAddress="0.0.0.0"
proxyPort=8090
socksParentProxy = "127.0.0.1:1081"
socksProxyType = socks5
diskCacheRoot = ""

export http_proxy=http://localhost:8090
export https_proxy=http://localhost:8090
git config --global http.proxy http://127.0.0.1:8090
git config --global https.proxy http://127.0.0.1:8090

sslocal -c /etc/shadowsocks.json &
polipo -c /etc/polipo.conf


# 取消 git 代理
git config --global --unset http.proxy
git config --global --unset https.proxy
