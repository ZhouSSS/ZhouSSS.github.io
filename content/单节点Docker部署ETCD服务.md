---
title: 单节点Docker部署ETCD服务
date: 2024-09-29T21:31:50+08:00
categories: [云计算,Docker]
tags: [中间件]
---

## 构建etcd服务端容器

``` bash
docker run -d --name=etcd-server \
-e ALLOW_NONE_AUTHENTICATION=yes \
bitnami/etcd:3.5 \
etcd
```

## 构建etcd客户端容器，用于测试
``` bash 
docker run -d --name=etcd-client \
-e ALLOW_NONE_AUTHENTICATION=yes \
bitnami/etcd:3.5 \
etcd
```

## 客户端容器内，操作etcd-server
```bash 
etcdctl --endpoints=http://172.17.0.2:2379 put /mykey "value"

etcdctl --endpoints=http://172.17.0.6:2379 get /mykey
# 获取全部以mykey为前缀的value
etcdctl --endpoints=http://172.17.0.6:2379 get /mykey --prefix
```

#### 代码块测试

```json
{
    "command":"engine_step",
    "args":{
        [
            "x":1,
            "y":2
        ]
    }
}
```

```cpp
#include  <stdio>
int func(int x,int y){
    return x*y;
}
int main(){
    double x = func(1,2);
    return 1;
}
```
```python

if __name__ == "main":
    print("hello")
    pass

```