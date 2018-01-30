# chart-elasticsearch
This repository contains 2 charts that are used to deploy elasticsearch to kubernetes.
- elasticsearch-storage
- elasticsearch

## Installing
First install `elasticsearch-storage` chart
```
helm install --name elasticsearch-storage chartmuseum/elasticsearch-storage
```

After that, install `elasticsearch` chart
```
helm install --name elasticsearch chartmuseum/elasticsearch
```