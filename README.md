# opt
监控特殊日志。如：[2015-12-23 16:44:03][/home/kds/trunk/server/src/vendor/common/stacker.go:9]|fatalstart ，需要docker和docker compose
```
当收集到这条日志，首先通过管道服“|”截取判断是否出现关键字“fatalstart”，出现则微信邮箱报警
```

### Structure
```
~/opt/htdocs/Dockerfiles
├── README.md
├── redis
│   └── Dockerfile
├── logstash
│   ├── Dockerfile
│   └── kds
│       ├── agent.conf
│       └── index.conf
├── docker-compose-yml

```         


### Installation docker
```
$ sudo apt-get update
$ sudo apt-get install docker-engine
$ sudo service docker start
```
### Installation docker compose
```
$ pip install docker-compose
$ curl -L https://github.com/docker/compose/releases/download/1.5.2/run.sh > /usr/local/bin/docker-compose
$ chmod +x /usr/local/bin/docker-compose
```
### runing
```
~/opt/htdocs/Dockerfiles 目录下
docker-compose build & docker-compose up -d 
```
