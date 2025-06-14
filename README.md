# TBC服务套件部署说明书

## 部署步骤

### 准备工作

1. 确保已安装Docker和Docker Compose
   ```
   docker --version
   docker compose --version
   ```

2. 拉取项目
   ```
   git clone https://github.com/TuringBitChain/selfapi
   cd selfapi
   ```


### 启动服务

1. 启动所有服务
   ```
   docker compose up -d
   ```

2. 查看服务启动情况
   ```
   docker compose ps
   ```

## 服务状态检查

### 1. TBCNode检查

进入容器并检查节点状态：
```
docker exec -it tbcnode /bin/bash
alias tbc-cli="/TBCNODE/bin/bitcoin-cli -conf=/TBCNODE/node.conf -datadir=/TBCNODE/node_data_dir"
tbc-cli getinfo
```

查看日志：
```
docker exec -it tbcnode /bin/bash
pm2 logs tbcd
```

查看区块同步状态：
```
docker exec -it tbcnode /bin/bash
alias tbc-cli="/TBCNODE/bin/bitcoin-cli -conf=/TBCNODE/node.conf -datadir=/TBCNODE/node_data_dir"
tbc-cli getblockcount
```


### 2. ElectrumX检查

查看ElectrumX日志：
```
docker logs electrumx --tail 100
```

查看实时日志：
```
docker logs --tail 100 -f electrumx
```

### 3. INDEX检查

查看API服务日志：
```
docker logs index --tail 100
```

### 4. TBCAPI检查

查看API服务日志：
```
docker logs goapi --tail 100
```

测试API是否启动：
```
curl http://localhost:5000/v1/tbc/main/health
```

## 常见问题排查

### 如果服务无法启动

检查容器错误日志：
```
docker logs [容器名称]
```


### 数据持久化确认

检查数据目录是否正常创建：
```
ls -la ./node_data
ls -la ./electrumx_data
ls -la ./mysqldata
```

## 服务更新

更新所有服务：
```
docker compose pull
docker compose up -d
```

更新特定服务：
```
docker compose pull [服务名]
docker compose up -d [服务名]
```


## 服务停止和清理

停止所有服务：
```
docker compose down
```

停止服务并删除网络（保留数据）：
```
docker compose down --remove-orphans
```

完全清理（慎用，会删除所有数据）：
```
docker compose down -v --remove-orphans
```

## 注意事项

1.INDEX重启需要重新建立数据库，比较浪费时间（大约1个小时）



