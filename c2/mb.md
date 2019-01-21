# 2.5 MetricBeat配置
## 下载安装
目前最新版本是6.5.4。   
### 下载可执行文件
#### 64位linux操作系统
```bash
wget https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-6.5.4-linux-x86_64.tar.gz
tar xf metricbeat-6.5.4-linux-x86_64.tar.gz
mv metricbeat-6.5.4-linux-x86_64 ./metricbeat
```
那么当前目录下面metricbeat文件夹里面就是全部了，包含可执行文件和配置文件
#### MAC
```bash
wget https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-6.5.4-darwin-x86_64.tar.gz
```
当然也可以使用brew来安装
```bash
brew install metricbeat
```
那么配置文件位于： **/usr/local/etc/metricbeat/metricbeat.yml**
#### 64位Windows
```
https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-6.5.4-windows-x86_64.zip
```
#### 64位Debian
```bash
wget https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-6.5.4-amd64.deb
dpkg -i metricbeat-6.5.4-amd64.deb
systemctl start metricbeat
```
配置文件位于： **/etc/metricbeat/metricbeat.yml**
#### 64位Centos
```bash
wget https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-6.5.4-x86_64.rpm
rpm -ivh metricbeat-6.5.4-x86_64.rpm
systemctl start metricbeat
```
配置文件位于： **/etc/metricbeat/metricbeat.yml**

Debian，Ubuntu，Centos等操作系统推荐采用后面的安装方式，这样Systemd会自动安装好，方便快速管理。上面下载地址中包含了版本，如果读者希望下载其他版本，那么只需要将数字6.5.4进行替换即可。   
## 配置文件结构
```
├── fields.yml
├── metricbeat.reference.yml
├── metricbeat.yml
└── modules.d
    ├── aerospike.yml.disabled
    ├── apache.yml.disabled
    ├── ceph.yml.disabled
    ├── couchbase.yml.disabled
    ├── docker.yml.disabled
    ├── dropwizard.yml.disabled
    ├── elasticsearch.yml.disabled
    ├── etcd.yml.disabled
    ├── golang.yml.disabled
    ├── graphite.yml.disabled
    ├── haproxy.yml.disabled
    ├── http.yml.disabled
    ├── jolokia.yml.disabled
    ├── kafka.yml.disabled
    ├── kibana.yml.disabled
    ├── kubernetes.yml.disabled
    ├── logstash.yml.disabled
    ├── memcached.yml.disabled
    ├── mongodb.yml.disabled
    ├── mysql.yml.disabled
    ├── nginx.yml.disabled
    ├── php_fpm.yml.disabled
    ├── postgresql.yml.disabled
    ├── prometheus.yml.disabled
    ├── rabbitmq.yml.disabled
    ├── redis.yml.disabled
    ├── system.yml
    ├── uwsgi.yml.disabled
    ├── vsphere.yml.disabled
    ├── windows.yml.disabled
    └── zookeeper.yml.disabled
```
其中需要关注的文件有metricbeat.yml和modules.d下的模块监控文件。我们可以看到metricbeat已经支持绝大多数的中间件，方便我们快速监控相关组件。   
如果需要启动某个模块的监控只需要将相关模块配置文件从xxxx.yml.disabled修改成xxxx.yml或者也是推荐的做法**metricbeat modules enable xxxx**
接下来我们将讲述相关的一些yml文件配置项
### metricbeat.yml
```yaml
# 模块配置信息
metricbeat.config.modules:
  # 模块配置文件的路径
  path: ${path.config}/modules.d/*.yml
  # 是否自动加载发生变化的配置文件
  reload.enabled: true
  # 自动重载的检测周期
  reload.period: 10s
# ES存储中相关设置
setup.template.settings:
  # 索引分片数量
  index.number_of_shards: 1
  # 索引的压缩
  index.codec: best_compression
  # 是否保留_source字段
  #_source.enabled: false
# 当前beat的名字 读取方式：data['beat']['name']
name: test
# 标签
#tags: ["service-X", "web-tier"]
# 其他字段 读取方式：data['fields']['env']
#fields:
#  env: staging
# 是否自动设置kibana看板
setup.dashboards.enabled: true
# 看板的下载地址，一般不用设置，使用elatic.co官方的即可
#setup.dashboards.url:

#============================== Kibana =====================================

# Starting with Beats version 6.0.0, the dashboards are loaded via the Kibana API.
# This requires a Kibana endpoint configuration.
setup.kibana:

  # Kibana Host
  # Scheme and port can be left out and will be set to the default (http and 5601)
  # In case you specify and additional path, the scheme is required: http://localhost:5601/path
  # IPv6 addresses should always be defined as: https://[2001:db8::1]:5601
  #host: "localhost:5601"

#============================= Elastic Cloud ==================================

# These settings simplify using metricbeat with the Elastic Cloud (https://cloud.elastic.co/).

# The cloud.id setting overwrites the `output.elasticsearch.hosts` and
# `setup.kibana.host` options.
# You can find the `cloud.id` in the Elastic Cloud web UI.
#cloud.id:

# The cloud.auth setting overwrites the `output.elasticsearch.username` and
# `output.elasticsearch.password` settings. The format is `<user>:<pass>`.
#cloud.auth:

#================================ Outputs =====================================

# Configure what output to use when sending the data collected by the beat.

#-------------------------- Elasticsearch output ------------------------------
output.elasticsearch:
  # Array of hosts to connect to.
  hosts: ["localhost:9200"]

  # Optional protocol and basic auth credentials.
  #protocol: "https"
  #username: "elastic"
  #password: "changeme"

#----------------------------- Logstash output --------------------------------
#output.logstash:
  # The Logstash hosts
  #hosts: ["localhost:5044"]

  # Optional SSL. By default is off.
  # List of root certificates for HTTPS server verifications
  #ssl.certificate_authorities: ["/etc/pki/root/ca.pem"]

  # Certificate for SSL client authentication
  #ssl.certificate: "/etc/pki/client/cert.pem"

  # Client Certificate Key
  #ssl.key: "/etc/pki/client/cert.key"

#================================ Logging =====================================

# Sets log level. The default log level is info.
# Available log levels are: error, warning, info, debug
#logging.level: debug

# At debug level, you can selectively enable logging only for some components.
# To enable all selectors use ["*"]. Examples of other selectors are "beat",
# "publish", "service".
#logging.selectors: ["*"]

#============================== Xpack Monitoring ===============================
# metricbeat can export internal metrics to a central Elasticsearch monitoring
# cluster.  This requires xpack monitoring to be enabled in Elasticsearch.  The
# reporting is disabled by default.

# Set to true to enable the monitoring reporter.
#xpack.monitoring.enabled: false

# Uncomment to send the metrics to Elasticsearch. Most settings from the
# Elasticsearch output are accepted here as well. Any setting that is not set is
# automatically inherited from the Elasticsearch output configuration, so if you
# have the Elasticsearch output configured, you can simply uncomment the
# following line.
#xpack.monitoring.elasticsearch:
```