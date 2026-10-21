---
type: 'BigData'
collections: '20261'
title: '#4 Data Lake Zoning'
createdAt: '2026-08-03T07:35:54.117Z'
lastUpdated: '2026-10-01T02:55:52.607Z'
tags: []
url: 'https://app.capacities.io/home/5c45e3d8-1e9c-4a75-8c3a-defcc807d3b3'
---

# #4 Data Lake Zoning

![image](https://capacities-files.s3.eu-central-1.amazonaws.com/private/7b8e6a82-d5b2-427a-b793-da94511f386f/raw.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIA5VTNRR6EBR56K2NK%2F20261001%2Feu-central-1%2Fs3%2Faws4_request&X-Amz-Date=20261001T013112Z&X-Amz-Expires=43200&X-Amz-Signature=fb36ae795fe71bf6ba1b2802d1abf9f18a878223f727b8adccf6f8984f65e3d1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
[image - Notes](https://app.capacities.io/e288d27f-67f3-4518-a8cf-7caaffd5e145/7b8e6a82-d5b2-427a-b793-da94511f386f)

# Raw Zone (Bronze)

The Bronze layer stores raw data from various sources in its native structure, serving as a historical record and a reliable initial storage

# Curated Zone (Silver)

The Silver layer refines and standardizes raw data for complex analytics through quality checks, standardization, deduplication, and other transformations. It acts as a transitional stage for processed, granular data with improved quality and consistency.

# Consumption zone (Gold)

The Gold layer optimizes refined data for specific business insights and decisions. It aggregates, summarizes, and enriches data for high-level reporting and analytics, emphasizing performance and scalability to provide fast access to key metrics and insights

# Labs

```python
  seaweedfs:
    image: chrislusf/seaweedfs
    container_name: seaweedfs
    ports:
      - "8333:8333" # API
      - "9333:9333" # UI Cluster
      - "7777:8888" # UI Filler
    volumes:
      - seaweedfs:/data
    environment:
      - AWS_ACCESS_KEY_ID=admin
      - AWS_SECRET_ACCESS_KEY=secret
    command: server -dir=/data -s3 -filer
    networks:
      - tbd_network
volumes:
  seaweedfs:
```


```yaml
  jupyter:
    image: quay.io/jupyter/minimal-notebook:latest
    container_name: jupyter
    hostname: jupyter
    ports:
      - "8888:8888"
    environment:
      - JUPYTER_ENABLE_LAB=yes
      - JUPYTER_TOKEN=secretsecret
    volumes:
      - ./jupyter:/home/jovyan/work
    networks:
      - tbd_network
```


```python
!pip install boto3 pandas pyarrow fsspec s3fs
```

```python
# import io
# import json
import boto3
import pandas as pd
# from botocore.client import Config

s3 = boto3.client(
    "s3",
    endpoint_url="http://seaweedfs:8333",
    aws_access_key_id="admin",
    aws_secret_access_key="secret"
)
```

```python
s3.create_bucket(Bucket="bronze")
s3.create_bucket(Bucket="silver")
s3.create_bucket(Bucket="gold")
```

```python
# Producer to Bronze
s3.upload_file(
    Filename='sales.csv',
    Bucket='bronze',
    Key='sales.csv'
)
```

```python
# Bronze to Silver
storage_options = {
    "key": "admin",
    "secret": "secret",
    "client_kwargs": {
        "endpoint_url": "http://seaweedfs:8333"
    }
}
s3_path = f"s3://bronze/sales.csv"
df = pd.read_csv(s3_path, storage_options=storage_options)
df_clean = df[(df['qty'] != 0) | (df['price'] != 0)]
silver_path = "s3://silver/sales.parquet"
df_clean.to_parquet(silver_path, engine='pyarrow', compression='snappy', storage_options=storage_options)
```

```python
# Silver to Gold
df_silver = pd.read_parquet(
    silver_path, 
    engine='pyarrow', 
    storage_options=storage_options
)
df_grouped = df_silver.groupby('store').agg({
    'qty': 'sum',
    'id': 'count'
}).rename(columns={'id': 'total_transaksi'}).reset_index()
df_grouped
gold_path = "s3://gold/sales.parquet"
df_clean.to_parquet(gold_path, engine='pyarrow', compression='snappy', storage_options=storage_options)
```

# References

Strengholt, P. (2025). *Building Medallion Architectures: Designing with Delta Lake and Spark*. " O'Reilly Media, Inc.".

