# CentOS 7一键安装Seafile
Seafile 是一款开源的企业云盘，和Owncloud类似，个人感觉Seafile更加稳定，搭建也很简单，写了一个一键脚本，方便安装。（一键脚本使用SqLite 3作为数据库）
___

### 环境要求
* CentOS 7 64位
* Python >= 2.7
* SqLite 3

### 开始安装
```bash
#已经安装wget这一步可省略
yum -y install wget
wget https://raw.githubusercontent.com/helloxz/seafile/master/install_seafile.sh
chmod +x install_seafile.sh && ./install_seafile.sh
```

详细说明请访问：[https://www.xiaoz.me/archives/8480](https://www.xiaoz.me/archives/8480)

# debian 一键安装方法和升级到7.0版本
环境预备
正式安装之前，需要先安装一众的应用，包括

python 2.7
python-setuptools
python-imaging
python-ldap
python-urllib3
sqlite3

执行命令
```bash
apt-get update
apt-get upgrade
apt-get install python2.7 libpython2.7 python-setuptools python-imaging python-ldap python-urllib3 sqlite3
```
## 一键安装Seafile
我们先下载一键包，这里安装的是6.2.3版本，执行命令：
```bash
wget https://raw.githubusercontent.com/helloxz/seafile/master/install_seafile.sh
chmod +x install_seafile.sh && ./install_seafile.sh
```

正常情况下，系统会显示安装、卸载和退出。第一次安装，选择1即可。如果已经安装失败，需要先选择2卸载一次再重新安装。

安装过程比较简单，根据提示填写名称、账户、密码和端口即可。

首次安装之后，系统会默认访问端口为8000。如果过程中没有提示新建邮箱和账户，建议重启VPS，然后进入/home/MyCloud/seafile-server目录，启动Seafile和Seahub。
```bash
./seafile.sh start
./seahub.sh start
```
## 升级安装7.0

1、把服务端下下来先, 不好意思，这里应该没有安装源，麻烦自己下载到服务器，放在之前一键安装的目录/home/MyCloud/下面

```bash
wget http://seafile-downloads.oss-cn-shanghai.aliyuncs.com/seafile-server_7.0.5_x86-64.tar
```

2、 解压缩
```bash
tar -zxvf seafile-server_7.0.5_x86-64.tar
```

3、停止seafile服务和seahub
```bash
./seafile.sh stop
./seahub.sh stop
```

4、文件夹改名
```bash
mv seafile-server seafile-server-6.2.3
mv seafile-server_7.0.5 seafile-server
```

5、修改文件夹权限
```bash
chmod -R 755 /home/MyCloud/seafile-server/
```
6、更新版本，将6.2更新到6.3
```bash
cd /home/MyCloud/seafile-server/upgrade/
./upgrade_6.2_6.3.sh
```

到这里为止更新完毕！！！重新启动seafile和seahub即可，如果一切正常后，可以重新执行第5步，将6.3更新到7.0.

```bash
cd /home/MyCloud/seafile-server/upgrade/
./upgrade_6.3_7.0.sh
```
要注意！更新前要停止seafile和seahub.

## 另外，更新后会出现服务器无法连接问题，这是因为7.0版本之后，8000端口默认监听在127.0.0.1上，这意味着无法直接通过8000端口访问Seafile服务。
### 解决办法：找到服务器上Seafile安装路径，在\conf\gunicom.conf中把bind = “127.0.0.1:8000” 改成 “0.0.0.0:8000″。然后重启seafile和seahub服务即可解决。


####本人纯属小白新人，写这个主要是给自己做个记录，有写的不好的，请大神指教！！

## 最后，感谢helloxz大神制作一脚脚本，为我们小白谋福利
