卷开启/关闭分布式缓（remoteCacheEnable）：只有当集群和卷都开启了分布式缓存，分布式缓存才会对卷生效。

```
./cfs-cli vol update testflash --remoteCacheEnable true
```

设置remoteCacheReadTimeout:

```
// 从flashnode读请求的超时时间,默认100ms
./cfs-cli volume update testflash --remoteCacheReadTimeout=100
```

查询flashnode列表

```
// 列出所有flashnode的信息
./cfs-cli flashnode list
```


启用/禁用flashnode

```
./cfs-cli flashnode set 111:111:111:111:17510 true
./cfs-cli flashnode set 111:111:111:111:17510 false 
```

flashgroup操作

创建fg

```
// 创建后的flashgroup状态是inactive的，需要设置成active
// 支持权重参数配置, 通过--weight指定。参数必须大于0。缺省情况表示权重为1。
// fg的slot数量=权重值*32
./cfs-cli flashgroup create [--weight 2]
```

设置flashgroup  active
```
// 在创建flashgroup后，把flashgroup设置成active状态
./cfs-cli flashgroup set 25 true
```

flashgroup添加flashnode

```
./cfs-cli flashgroup nodeAdd 13 --zone-name=flashcache --addr="111:111:111:111:17510"
```

查看flashgroup列表

```
./cfs-cli flashgroup list
```

查看flashgroup关系表

```
./cfs-cli flashgroup graph
```