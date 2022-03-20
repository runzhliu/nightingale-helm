# n9e-helm

## Overview

[nightingale](https://github.com/didi/nightingale) Helm Charts，可以参考下面的部署命令进行部署，细节主要参考 values.yaml 文件

1. 因为 n9e 需要依赖 MySQL 和 Redis，所以提供了容器化的数据库来进行部署，同时也支持外部存储
2. 告警需要用到企业微信，告警脚本已经默认加上了代理，如果需要修改代理地址，需要多加一个选项，如 `--set notify.proxy=x.x.x.x:3128`
3. 配置企业微信机器人的token，如 `--set notify.wecomToken=xxx`

## 容器化存储

```bash
helm install n9e \
--namespace n9e --create-namespace \
--set redis.enabled=true \
--set mysql.initDb=true \
--set images.nserver=ulric2019/nightingale:5.4.1 \
--set images.nwebapi=ulric2019/nightingale:5.4.1 \
--set insideMysql.image=mysql:5.7 \
--set insideRedis.image=redis:6.2 \
--set insideMysql.enabled=true \
--set initMysql.enabled=true \
--set insideRedis.enabled=true \
./n9e
```

## 外部存储

```bash
helm install n9e \
--set outSideMysql.enabled=true \
--set outSideMysql.Address=mysql:3306 \
--set outSideMysql.User=root \
--set outSideMysql.Pwd=root \
--set initMysql.enabled=true \
--set outSideRedis.Address=redis:6379 \
--set images.nserver=ulric2019/nightingale:5.4.1 \
--set images.nwebapi=ulric2019/nightingale:5.4.1 \
./n9e
```
