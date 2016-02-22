
## 文件说明
Dockerfile文件

`
FROM 10.0.29.249:5000/redis
RUN echo "Asia/Shanghai" > /etc/timezone && dpkg-reconfigure -f noninteractive tzdata
EXPOSE 6379

`
- ｀FROM 10.0.29.249:5000/redis｀ ，`10.0.29.249:5000/redis` 是docker image名称，可以写`FROM redis` 从官网pull image。
- `RUN echo "Asia/Shanghai" > /etc/timezone && dpkg-reconfigure -f noninteractive tzdata` 保证docker容器的时间和宿主机一致
- `EXPOSE 6379` redis container对外开放6379端口
