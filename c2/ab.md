# 2.6 AuditBeat配置
## 下载安装
官方下载页面：https://www.elastic.co/downloads/beats/auditbeat，里面有各个版本，读者可以根据自己的系统选择不同的安装包。    
目前最新版本是6.6.1
#### MAC
```bash
brew install audibeat
```
或者下载DMG包。   
```bash
wget https://artifacts.elastic.co/downloads/beats/auditbeat/auditbeat-6.6.1-darwin-x86_64.tar.gz
tar xzvf auditbeat-6.6.1-darwin-x86_64.tar.gz
```
#### Debian或者Ubuntu
```bash
wget https://artifacts.elastic.co/downloads/beats/auditbeat/auditbeat-6.6.1-amd64.deb
dpkg -i auditbeat-6.6.1-amd64.deb
```
#### CentOS
```bash
wget https://artifacts.elastic.co/downloads/beats/auditbeat/auditbeat-6.6.1-x86_64.rpm
rpm -ivh auditbeat-6.6.1-x86_64.rpm
```
## 配置文件
默认目录：** /etc/auditbeat/ **

## 加载预设的模板
AuditBeat和其他Beats一样，预设了一堆ElasticSearch和Kibana的模板，这些文件定义在默认目录下的fields.yml。   
### 相关参数
在auditbeat.yml中有几个参数和预设模板有关。
```yaml
# 是否覆盖已经存在的模板
setup.template.overwrite: true
# 是否启用模板
setup.template.enabled: false
```
设置模板到ES中：   
```bash
auditbeat setup --dashboards
```
### 删除已有数据
```bash
curl -XDELETE 'http://localhost:9200/auditbeat-*'
```
### 模板导出
```bash
auditbeat export template > auditbeat.template.json
```
### 模板导入
```bash
curl -XPUT -H 'Content-Type: application/json' http://es_host:9200/_template/auditbeat-6.6.1 -d@auditbeat.template.json
```
## 启动
```bash
systemctl start auditbeat
```
测试是否启动成功，也比较简单，首先可以查看进程，也可以查看日志**/var/log/auditbeat/auditbeat.log**，同时如果配置好了ElasticSearch的话，那么还可以查看ES的数据。   
```bash
curl -XGET 'http://localhost:9200/auditbeat-*/_search?pretty'
```