# rtlogging
beidou rtlogging data for ntrip and tcp
北斗数据流服务

::: danger 注意
专业版服务已经集成了北斗数据流服务，标准版没有数据流相关的，当你的项目中需要自己去实现处理北斗原始数据流相关的功能，可以自定义去实现，也可以集成我们的北斗数据流服务。支持Ntrip、Tcp、Mtrip等协议。
:::

## 镜像安装

Rtlogging为北斗数据流服务，使用 [docker-compose](https://docs.docker.com/compose/install) 脚本进行镜像安装

x86_64 架构使用镜像registry.cn-hangzhou.aliyuncs.com/navmg/rtlogging:xxxx

aarch_64 架构使用镜像registry.cn-hangzhou.aliyuncs.com/navmg/rtlogging-aarch64:xxxx

当前案例中版本的服务镜像为：`1.0.0`

docker-compose-rtlogging.yml脚本配置

```yaml
version: "3"

services:
  rtlogging-server:
    image: registry.cn-hangzhou.aliyuncs.com/navmg/rtlogging:1.0.0
    container_name: rtlogging-server
    hostname: rtlogging-server
    restart: always
    volumes:
      - ${RTLOGGING_LOGBACK}:/home/rtlogging/logback
      - ${RTLOGGING_RAW_PATH}:/app/raw
    ports:
      - ${RTLOGGING_PORT}:9964
      - ${RTLOGGING_CASTER_SERVER_PORT}:9090
      - ${RTLOGGING_CASTER_CLIENT_PORT}:9095
    environment:
      - RTLOGGING_CASTER_ENABLE=${RTLOGGING_CASTER_ENABLE}
    networks:
      - iot-net

networks:
  iot-net:
    driver: bridge
```



### 环境变量解释说明

::: tip 说明
`北斗数据流服务`默认的服务端口为9964，默认`RTLOGGING_CASTER_ENABLE`为true，默认的`RTLOGGING_CASTER_SERVER_PORT` ntrip caster server默认端口为9090，默认的`RTLOGGING_CASTER_CLIENT_PORT` ntrip caster client默认端口为9095
:::

| **变量**  | **说明**            |
|---------|-------------------|
| RTLOGGING_LOGBACK    | 服务日志文件根路径         |
| RTLOGGING_RAW_PATH | 原始观测文件存储根路径         |
| RTLOGGING_PORT | 北斗数据流服务映射的服务端口           |
| RTLOGGING_CASTER_SERVER_PORT | ntrip caster server映射端口         |
| RTLOGGING_CASTER_CLIENT_PORT | ntrip caster client映射端口         |


::: warning 特别说明
北斗数据流中的文件存储格式按照小时段进行存储文件的，文件时间按照GPS时分类文件夹的。
:::

