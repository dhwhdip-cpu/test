---
type: 'BigData'
collections: '20261'
title: '#9 Stream Processing'
createdAt: '2026-08-05T03:31:37.875Z'
lastUpdated: '2026-10-19T06:37:50.488Z'
tags: []
url: null
---

# #9 Stream Processing

# **Event-Driven Architecture & Clickstream**

- **Event-Driven:** Sistem bekerja berdasarkan "kejadian" (*event*), bukan jadwal baku. Setiap kali ada kejadian, sistem langsung bereaksi.

- **Studi Kasus Clickstream E-Commerce:** Setiap interaksi pengguna di aplikasi (seperti *login*, klik produk, memasukkan ke keranjang) adalah sebuah *event*. Analisis *clickstream* menangkap jejak aliran interaksi ini untuk memahami perilaku konsumen secara *real-time*.

# **Micro-batching vs Continuous Streaming**

- **Micro-batching (Apache Spark Streaming):**

- Data ditampung dulu dalam jeda waktu yang sangat singkat (misalnya per 1 detik), lalu diproses sebagai *batch* berukuran mini.

- *Analogi:* Kereta gantung yang menunggu keranjangnya penuh/waktunya tiba sebelum melaju.

- **Continuous Streaming (Apache Flink):**

- Data diproses satu per satu secara individual begitu data tersebut masuk, tanpa menunggu dikumpulkan.

- *Analogi:* Eskalator yang langsung membawa orang naik satu per satu.

# Labs

Jalankan container python, kafka, seaweedfs

```yaml
services:
  mongodb:
    image: mongo:latest
    container_name: mongodb_serving_layer
    ports:
      - "27017:27017"
    environment:
      MONGO_INITDB_ROOT_USERNAME: admin
      MONGO_INITDB_ROOT_PASSWORD: secretpassword
    volumes:
      - mongodb_data:/data/db
    restart: always
  python:
    build: ./python
    container_name: python
    volumes:
      - ./python:/app
    tty: true
    networks:
      - docker_network
  minio:
    image: minio/minio:latest
    container_name: minio
    hostname: minio
    ports:
      - "9000:9000"   # Port API S3
      - "9001:9001"   # Port Web Console
    environment:
      MINIO_ROOT_USER: admin
      MINIO_ROOT_PASSWORD: secretsecret
    volumes:
      - ./minio/data:/data1
    #   - ./minio/data2:/data2
    #   - ./minio/data3:/data3
    #   - ./minio/data4:/data4
    command: server /data --console-address ":9001"
    # command: server /data1 /data2 /data3 /data4 --console-address ":9001"
    restart: unless-stopped
    networks:
      - docker_network
  seaweedfs:
    image: chrislusf/seaweedfs
    container_name: seaweedfs
    ports:
      - "8333:8333" # API
      - "9333:9333" # Cluster UI
      - "7777:8888" # Filer UI
    volumes:
      - seaweed-data:/data
    environment:
      - AWS_ACCESS_KEY_ID=admin
      - AWS_SECRET_ACCESS_KEY=secret
    command: "server -dir=/data -filer -s3 -s3.port=8333 -volume.max=50 -master.volumeSizeLimitMB=200"
    networks:
      - docker_network
  jupyter:
    # image: quay.io/jupyter/minimal-notebook:latest
    build: ./jupyter
    container_name: jupyter
    ports:
      - "8888:8888"
    environment:
      - JUPYTER_ENABLE_LAB=yes
      - JUPYTER_TOKEN=secretsecret
    volumes:
      - ./jupyter:/home/jovyan/work
    networks:
      - docker_network 
  kafka:
    image: apache/kafka:latest
    container_name: kafka
    ports:
      - "9092:9092"
    environment:
      KAFKA_NODE_ID: 1
      KAFKA_PROCESS_ROLES: 'broker,controller'
      KAFKA_LISTENERS: 'INTERNAL://0.0.0.0:9092,EXTERNAL://0.0.0.0:9094,CONTROLLER://0.0.0.0:9093'
      KAFKA_ADVERTISED_LISTENERS: 'INTERNAL://kafka:9092,EXTERNAL://localhost:9094'
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: 'INTERNAL:PLAINTEXT,EXTERNAL:PLAINTEXT,CONTROLLER:PLAINTEXT'
      KAFKA_INTER_BROKER_LISTENER_NAME: 'INTERNAL'
      KAFKA_CONTROLLER_QUORUM_VOTERS: '1@kafka:9093'
      KAFKA_CONTROLLER_LISTENER_NAMES: 'CONTROLLER'
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
      KAFKA_TRANSACTION_STATE_LOG_REPLICATION_FACTOR: 1
      KAFKA_TRANSACTION_STATE_LOG_MIN_ISR: 1
    volumes:
      - ./kafka:/var/lib/kafka/data
    networks:
      - docker_network
  spark:
    image: bitnami/spark:3.5.0
    container_name: spark
    hostname: spark
    environment:
      - SPARK_MODE=master
      - SPARK_RPC_AUTHENTICATION_ENABLED=no
      - SPARK_RPC_ENCRYPTION_ENABLED=no
      - SPARK_LOCAL_STORAGE_ENCRYPTION_ENABLED=no
      - SPARK_SSL_ENABLED=no
    ports:
      - "8080:8080" # Spark UI
      - "7077:7077"
  qdrant:
    image: qdrant/qdrant:latest
    container_name: qdrant
    hostname: qdrant
    ports:
      - "6333:6333"  # HTTP REST API & Web Dashboard
      - "6334:6334"  # gRPC Engine
    volumes:
      - ./docker_volumes/qdrant:/qdrant/storage:z
    networks:
      - docker_network
  postgres:
    image: postgres:15-alpine
    container_name: postgres
    environment:
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: secretsecret
    ports:
      - "5432:5432"
    volumes:
      - pgdata:/var/lib/postgresql/data
    networks:
      - docker_network
    command:
      - postgres
      - -c
      - wal_level=logical
      - -c
      - max_replication_slots=10
      - -c
      - max_wal_senders=10
  debezium:
    image: quay.io/debezium/connect:3.2
    container_name: debezium
    ports:
      - "8083:8083"
    depends_on:
      - kafka
      - postgres
      - seaweedfs
      - nifi
    environment:
      BOOTSTRAP_SERVERS: kafka:9092
      GROUP_ID: 1
      CONFIG_STORAGE_TOPIC: cdc_connect_configs
      OFFSET_STORAGE_TOPIC: cdc_connect_offsets
      STATUS_STORAGE_TOPIC: cdc_connect_statuses
    networks:
      - docker_network
  nifi:
    image: apache/nifi:2.6.0
    container_name: nifi
    ports:
      - "8443:8443"
    environment:
      SINGLE_USER_CREDENTIALS_USERNAME: admin
      SINGLE_USER_CREDENTIALS_PASSWORD: "AdminNifi123456!"
      NIFI_JVM_HEAP_INIT: 256m
      NIFI_JVM_HEAP_MAX: 768m
    networks:
      - docker_network
    volumes:
      - nifi-conf:/opt/nifi/nifi-current/conf
      - nifi-state:/opt/nifi/nifi-current/state
      - nifi-database:/opt/nifi/nifi-current/database 
  cloudbeaver:
    image: dbeaver/cloudbeaver:24.0.0
    container_name: cloudbeaver
    ports:
      - "8080:8978"
    volumes:
      - ./cloudbeaver:/opt/cloudbeaver/workspace
    networks:
      - docker_network

volumes:
  nifi-conf:
  nifi-state:
  nifi-database:
  pgdata:
  mongodb_data:
  seaweed-data:
  
networks:
  docker_network:
    driver: bridge

```


melihat daftar topic di kafka

```shell
docker exec kafka /opt/kafka/bin/kafka-topics.sh --bootstrap-server localhost:9094 --list
```

buat topic

```shell
docker exec kafka /opt/kafka/bin/kafka-topics.sh --bootstrap-server localhost:9094 --create --topic pesanan
```

`consumer.py`

```python
import json
import boto3
from confluent_kafka import Consumer

s3 = boto3.client(
    "s3",
    endpoint_url="http://seaweedfs:8333",
    aws_access_key_id="admin",
    aws_secret_access_key="secret",
    region_name="us-east-1",
)
try:
    s3.create_bucket(Bucket="gold")
except Exception:
    pass 

consumer = Consumer({
    "bootstrap.servers": "kafka:9092",
    "group.id": "kasir-konsumen",
    "auto.offset.reset": "earliest",
})
consumer.subscribe(["pesanan"])

total = {}
diproses = 0

# Processing
while True:
    pesan = consumer.poll(1.0)
    if pesan is None or pesan.error():
        continue

    pesanan = json.loads(pesan.value())
    menu = pesanan["menu"]
    total[menu] = total.get(menu, 0) + pesanan["jumlah"]
    diproses += 1
    print("Terima:", pesanan, "-> total sekarang:", total)

    if diproses % 10 == 0:
        # simpan hasil ke SeaweedFS setiap 10 pesanan
        s3.put_object(
            Bucket="gold",
            Key="total_penjualan.json",
            Body=json.dumps(total, indent=2),
        )
        print(">>> Hasil disimpan ke SeaweedFS")
```

`producer.py`

```python
import json
import random
import time

from confluent_kafka import Producer

MENU = ["nasi goreng", "mie ayam", "es teh"]

producer = Producer({"bootstrap.servers": "kafka:9092"})

nomor = 0
while True:
    nomor += 1
    pesanan = {
        "nomor": nomor,
        "menu": random.choice(MENU),
        "jumlah": random.randint(1, 3),
    }
    producer.produce("pesanan", value=json.dumps(pesanan))
    producer.flush()
    print("Kirim:", pesanan)
    time.sleep(1)
```

`result.py`

```python
import boto3

s3 = boto3.client(
    "s3",
    endpoint_url="http://seaweedfs:8333",
    aws_access_key_id="admin",
    aws_secret_access_key="secret",
    region_name="us-east-1",
)

file = s3.get_object(Bucket="gold", Key="total_penjualan.json")
print(file["Body"].read().decode())
```



