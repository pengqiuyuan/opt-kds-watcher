# Run Under Ubuntu

## 环境要求

- Docker
- Docker compose

## 修改配置
elasticsearch目录
```
redis:
    build: ./redis
    ports:
      - "6378:6379"
    restart: always
logstashindexerkds:
    build: ./logstash
    volumes:
        - ~/opt-kds-watcher/htdocs/Dockerfiles/logstash/kds/index.conf:/opt/logstash/conf/index.conf
    command: logstash agent  -f /opt/logstash/conf/index.conf
    restart: always
logstashagentkds:
    build: ./logstash
    volumes:
        - ~/sincedb/sincedb_opt_kds_warcher:/opt/sincedb
        - /home/ftp/logs:/home/ftp/logs
        - ~/opt-kds-watcher/htdocs/Dockerfiles/logstash/kds/agent.conf:/opt/logstash/conf/agent.conf
    command: logstash agent  -f /opt/logstash/conf/agent.conf 
    restart: always
```
- redis：日志消息队列
- logstashindexerkds：处理日志，发送报警邮件
- logstashagentkds：收集宿主机`/home/ftp/logs`目录下面的日志，发送到redis。`~/sincedb/sincedb_opt_kds_warcher:/opt/sincedb`记录日志的生产位置，挂载到宿主机的`~/sincedb/sincedb_opt_kds_warcher`目录下，`/opt/sincedb`是容器的日志位置记录文件，第一次自己记录生成，容器被删除以后，加载宿主机`~/sincedb/sincedb_opt_kds_warcher`下的文件

