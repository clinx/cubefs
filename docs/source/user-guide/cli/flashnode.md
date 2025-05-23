Volume-level Enable/Disable Distributed Cache (remoteCacheEnable):

Distributed caching takes effect for a volume only if both the cluster-level caching function and the volume-specific remoteCacheEnable flag are set to enabled.

```
./cfs-cli vol update testflash --remoteCacheEnable true
```

Set remoteCacheReadTimeout:

```
// Specifies the timeout duration for read operations from FlashNode. The default value is 100 milliseconds.
./cfs-cli volume update testflash --remoteCacheReadTimeout=100
```

List all FlashNodes

```
./cfs-cli flashnode list
```

Enable/Disable FlashNode

```
./cfs-cli flashnode set 111:111:111:111:17510 true
./cfs-cli flashnode set 111:111:111:111:17510 false 
```

Management Operations for FlashGroup

create FlashGroup

```
// A newly created FlashGroup is in the inactive state by default and must be manually activated.
// This operation supports configuring the weight parameter using the --weight option.
// The weight value must be greater than zero; if not specified, it defaults to 1.
// The total number of slots assigned to the FlashGroup is calculated as weight * 32.
./cfs-cli flashgroup create [--weight 2]
```

set flashgroup  active

```
// Activate the FlashGroup after its creation
./cfs-cli flashgroup set 25 true
```

flashgroup add flashnode

```
./cfs-cli flashgroup nodeAdd 13 --zone-name=flashcache --addr="111:111:111:111:17510"
```

show flashgroups info

```
./cfs-cli flashgroup list
```

show flashgroup graph

```
./cfs-cli flashgroup graph
```