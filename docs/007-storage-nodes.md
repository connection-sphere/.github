# Storage Nodes

**Storage Nodes** are dedicated servers designed to provide private, scalable object storage, reducing our dependency on external providers such as AWS S3. 

Their primary purpose is to store and serve files efficiently while lowering storage costs.

## Installation

```bash
export OPSLIB=~/code1/secret/product01ion && \
saas install --node=storage01 --root && \
saas deploy --node=storage01 && \
saas start --node=storage01 --root
```

## Accessing 

[https://strg01.connectionsphere.com](https://strg01.connectionsphere.com)


