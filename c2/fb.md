# 2.4 FileBeat配置
目前最新版本6.6.2，[官方下载页面](https://www.elastic.co/downloads/beats/filebeat)
## 下载安装
### Debian && Ubuntu
```bash
wget https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-6.6.2-amd64.deb
dpkg -i filebeat-6.6.2-amd64.deb
```
### Centos
```bash
wget https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-6.6.2-x86_64.rpm
rpm -ivh filebeat-6.6.2-x86_64.rpm
```
### 其他Linux
```bash
wget https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-6.6.2-linux-x86_64.tar.gz
tar xf filebeat-6.6.2-linux-x86_64.tar.gz
```

建议使用deb或者rpm方式进行安装，这样安装完成后，系统后台任务也会安装好。   
默认配置文件位于：**/etc/filebeat**.   
自己解压的方式，默认配置文件位于当前目录下。   
## 常用命令
### 启动、停止
```bash
systemctl start filebeat
systemctl stop filebeat
systemctl restart filebeat
```
### 模块相关
```bash
# 列举系统所有支持的模块
filebeat modules list
# 启用某个模块
filebeat modules enable redis
# 禁用某个模块
filebeat modules disable redis
```
模块相关的配置信息位于：**/etc/filebeat/modules.d/**目录下。