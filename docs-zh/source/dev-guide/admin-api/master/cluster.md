# 集群管理

## 概述

调用资源管理节点 Master 提供的 API 进行集群管理。curl 命令中的 IP 和端口地址分别为资源管理节点配置文件中的 ip 和 listen 配置。

``` bash
curl -v "http://10.196.59.198:17010/admin/getCluster" | python -m json.tool
```

展示集群基本信息，比如集群包含哪些数据节点和元数据节点，卷等。

响应示例

``` json
{
 "code":0,
 "data":{
     "Applied":886268,
     "BadMetaPartitionIDs":[

     ],
     "BadPartitionIDs":[

     ],
     "DataNodeStatInfo":{

     },
     "DataNodes":[

     ],
     "DisableAutoAlloc":false,
     "LeaderAddr":"127.0.0.1:17010",
     "MaxDataPartitionID":735,
     "MaxMetaNodeID":57,
     "MaxMetaPartitionID":59,
     "MetaNodeStatInfo":{

     },
     "MetaNodeThreshold":0.75,
     "MetaNodes":[

     ],
     "Name":"cluster",
     "VolStatInfo":[

     ]
 },
 "msg":"success"
}
```

## 冻结集群

``` bash
curl -v "http://10.196.59.198:17010/cluster/freeze?enable=true"
```

如果启用了冻结集群功能，卷就不再自动地创建数据分片，也不能手动创建分片。

参数列表

| 参数     | 类型   | 描述               |
|--------|------|------------------|
| enable | bool | 如果设置为 true，则集群被冻结 |

## 获取集群空间信息

``` bash
curl -v "http://10.196.59.198:17010/cluster/stat"
```

按区域展示集群的空间信息。

响应示例

``` json
{
    "DataNodeStatInfo": {
        "TotalGB": 1,
        "UsedGB": 0,
        "IncreasedGB": -2,
        "UsedRatio": "0.0"
    },
    "MetaNodeStatInfo": {
        "TotalGB": 1,
        "UsedGB": 0,
        "IncreasedGB": -8,
        "UsedRatio": "0.0"
    },
    "ZoneStatInfo": {
        "zone1": {
            "DataNodeStat": {
                "TotalGB": 1,
                "UsedGB": 0,
                "AvailGB": 0,
                "UsedRatio": 0,
                "TotalNodes": 0,
                "WritableNodes": 0
            },
            "MetaNodeStat": {
                "TotalGB": 1,
                "UsedGB": 0,
                "AvailGB": 0,
                "UsedRatio": 0,
                "TotalNodes": 0,
                "WritableNodes": 0
            }
        }
    }
}
```

## 获取集群的拓扑信息

``` bash
curl -v "http://10.196.59.198:17010/topo/get"
```

按区域展示集群的拓扑信息。

响应示例

``` json
[
    {
        "Name": "zone1",
        "Status": "available",
        "NodeSet": {
            "700": {
                "DataNodeLen": 0,
                "MetaNodeLen": 0,
                "MetaNodes": [],
                "DataNodes": []
            }
        }
    },
    {
        "Name": "zone2",
        "Status": "available",
        "NodeSet": {
            "800": {
                "DataNodeLen": 0,
                "MetaNodeLen": 0,
                "MetaNodes": [],
                "DataNodes": []
            }
        }
    }
]
```

## 更新可用区状态

``` bash
curl -v "http://10.196.59.198:17010/zone/update?name=zone1&enable=false"
```

更新可用区的状态为可用或不可用。

参数列表

| 参数     | 类型     | 描述                 |
|--------|--------|--------------------|
| name   | string | 可用区名称              |
| enable | bool   | true 表示可用，false 为不可用 |

## 获取所有可用区信息

``` bash
curl -v "http://10.196.59.198:17010/zone/list"
```

获取所有可用区的名称及可用状态。

响应示例

``` json
[
    {
        "Name": "zone1",
        "Status": "available",
        "NodeSet": {}
    },
    {
        "Name": "zone2",
        "Status": "available",
        "NodeSet": {}
    }
]
```

## 获取集群信息

``` bash
curl -v "http://192.168.0.11:17010/admin/getNodeInfo"
```

获取集群信息

响应示例

``` json
{
    "code": 0,
    "data": {
        "autoRepairRate": "0",
        "batchCount": "0",
        "deleteWorkerSleepMs": "0",
        "loadFactor": "0",
        "maxDpCntLimit":"0",
        "markDeleteRate": "0"
    },
    "msg": "success"
}
```

## 设置集群信息

``` bash
curl -v "http://192.168.0.11:17010/admin/setNodeInfo?batchCount=100&markDeleteRate=100"
```

设置集群信息

参数列表

| 参数                  | 类型     | 描述                          |
|---------------------|--------|-----------------------------|
| batchCount          | uint64 | metanode 删除批量大小             |
| markDeleteRate      | uint64 | datanode 批量删除限速设置. 0 代表未做限速设置 |
| autoRepairRate      | uint64 | datanode 上同时修复的 extent 个数      |
| deleteWorkerSleepMs | uint64 | 删除间隔时间                      |
| loadFactor          | uint64 | 集群超卖比，默认 0，不限制               |
| maxDpCntLimit       | uint64 | 每个节点上 dp 最大数量，默认 3000， 0 代表默认值 |
