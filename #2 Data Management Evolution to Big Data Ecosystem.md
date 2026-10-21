---
type: 'BigData'
collections: '20261'
title: '#2 Data Management Evolution to Big Data Ecosystem'
createdAt: '2026-08-05T03:28:55.754Z'
lastUpdated: '2026-09-21T09:20:20.484Z'
tags: []
url: 'https://app.capacities.io/home/028a336f-2585-484d-8d00-d1e2b8958555'
---

# #2 Data Management Evolution to Big Data Ecosystem

# Data Maturity

---

Digital transformation involves shifting from manual to digital processes using technology and data such as machine learning to drive efficiency and business value. Enterprise data maturity encompasses four stages that describe an organization's capability to manage and extract value from data, serving as a benchmark for readiness to adopt advanced analytics and artificial intelligence for strategic decision-making.


---

![image](https://capacities-files.s3.eu-central-1.amazonaws.com/private/e78e1101-e2f6-42cd-8166-f8ba5ad35196/raw.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIA5VTNRR6EBR56K2NK%2F20260921%2Feu-central-1%2Fs3%2Faws4_request&X-Amz-Date=20260921T090944Z&X-Amz-Expires=43200&X-Amz-Signature=6dfe397aadfaf5c482a89ebf8b4993179a0ef4a812347178246fff2f151f2a70&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
[image - Notes](https://app.capacities.io/e288d27f-67f3-4518-a8cf-7caaffd5e145/e78e1101-e2f6-42cd-8166-f8ba5ad35196)

---

---

## Stage 1 Reactive

A company has data scattered all over, likely in a bunch of Excel **spreadsheets** and/or desktop databases on many different filesystems, being emailed all over the place. Data architects call this a spreadmart (short for “spreadsheet data mart”): an informal, **decentralized** **collection of data** often found within an organization that uses spreadsheets **to store, manage, and analyze data**.


---

## Stage 2 Informative

Companies reach the second maturity stage when they start to **centralize** their data, **making analysis and reporting much easier**. Stages 1 and 2 are for historical reporting, or seeing trends and patterns from the past


---

---

## Stage 3 Predictive

Companies have **moved to the cloud** and have built a system that can handle larger quantities of data, different types of data, and data that is ingested more frequently (hourly or streaming).


---

## Stage 4 Transformative

The company has built a solution that can **handle any data, no matter its size, speed, or type**. It is easy to onboard new data with a shortened lead time because the architecture can handle it and has the infrastructure capacity to support it.


---

# Data Architectures

## Relational Data Warehouse

---

A relational data warehouse (RDW) is a specialized relational database optimized for large-scale analytics, **integrating structured storage** (tables, rows, and columns) **with a closely coupled compute engine**, featuring robust transaction support, audit trails, and schema enforcement. It serves to centralize data from multiple applications and solves performance bottlenecks in operational systems (OLTP) caused by heavy CRUD queries, by copying operational data into the RDW so that analysis and reporting can run smoothly without burdening the main system.


---

![image](https://capacities-files.s3.eu-central-1.amazonaws.com/private/dc818876-9ebb-40a5-9a96-527da0ef24f1/raw.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIA5VTNRR6EBR56K2NK%2F20260921%2Feu-central-1%2Fs3%2Faws4_request&X-Amz-Date=20260921T090944Z&X-Amz-Expires=43200&X-Amz-Signature=13f030f0d6559099418fc4979eaac75d589892508bb00bff8331287fd0ec99cd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
[image - Notes](https://app.capacities.io/e288d27f-67f3-4518-a8cf-7caaffd5e145/dc818876-9ebb-40a5-9a96-527da0ef24f1)

---

## Data Lake

---

Data lakes use object storage, which does not need the data to be structured into rows and columns, most data lakes now exist in the cloud. a data lake is **schema-on-read**, meaning that no up-front work is needed to put data in the data lake: it can be as simple as copying files into it, like you would with folders on your laptop. The data in a data lake is **stored in its natural (or raw) format**, meaning it can go from its source system into the data lake without being transformed into another format.


---

![image](https://capacities-files.s3.eu-central-1.amazonaws.com/private/b2084602-8d46-42ea-9c68-f57ca59eb2b3/raw.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIA5VTNRR6EBR56K2NK%2F20260921%2Feu-central-1%2Fs3%2Faws4_request&X-Amz-Date=20260921T090944Z&X-Amz-Expires=43200&X-Amz-Signature=99072a428f227e5c89c926c57b234fbd9b3c627ded6a1595e0473e855ea18152&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
[image - Notes](https://app.capacities.io/e288d27f-67f3-4518-a8cf-7caaffd5e145/b2084602-8d46-42ea-9c68-f57ca59eb2b3)

---

## Modern Data Warehouse

Relational data warehouses and data lakes, on their own, are simplistic architectures. They use only one technology to centralize the data, with few to no supporting products. When you use more technologies and products to support relational data warehouses or data lakes, they evolve into the architectures. Data lakes failed to replace relational data warehouses but still offered benefits for staging and preparing data. Why not have the advantages of both? Around 2011, many companies started building architectures that place data lakes side by side with relational data warehouses to form the data architecture we now call the modern data warehouse (MDW).**The modern in modern data warehouse refers to the use of newer technologies and approaches to data warehousing**.

![image](https://capacities-files.s3.eu-central-1.amazonaws.com/private/d9d62a10-aaae-4396-96f9-ad2825ccbc1c/raw.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIA5VTNRR6EBR56K2NK%2F20260921%2Feu-central-1%2Fs3%2Faws4_request&X-Amz-Date=20260921T090944Z&X-Amz-Expires=43200&X-Amz-Signature=017aa32505cc8a32e3b1cdf2c939a299f2dd40eab60a81320ca2fbeb5b240657&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
[image - Notes](https://app.capacities.io/e288d27f-67f3-4518-a8cf-7caaffd5e145/d9d62a10-aaae-4396-96f9-ad2825ccbc1c)

## Data Fabric

Data fabrics started to appear around 2016. You could think of the data fabric architecture as an evolution of the modern data warehouse architecture, with more technology added to source more data, secure it, and make it available. Also, improvements have been made to how the system ingests data, transforms, queries, searches, and access data. With all those additions, the system becomes a “fabric” a large framework that can ingest any sort of data.

![image](https://capacities-files.s3.eu-central-1.amazonaws.com/private/a0a0638b-bb28-4243-af56-1168c2f63256/raw.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIA5VTNRR6EBR56K2NK%2F20260921%2Feu-central-1%2Fs3%2Faws4_request&X-Amz-Date=20260921T090944Z&X-Amz-Expires=43200&X-Amz-Signature=2eb1998eeeca43ed655bdbf2062419756e1fa6cc0cf2b243511037f34ab3279e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
[image - Notes](https://app.capacities.io/e288d27f-67f3-4518-a8cf-7caaffd5e145/a0a0638b-bb28-4243-af56-1168c2f63256)

## Data Lakehouse

A data lakehouse is a modern data architecture a portmanteau (blend) of data lake and data warehouse popularized by Databricks since 2020 designed to manage all types of data within a single repository. Unlike older **data lakes**, this architecture is equipped **with a transactional software** layer such as Delta Lake, Apache Iceberg, or Apache Hudi, allowing it to function reliably like a relational database for various queries and reporting.

![image](https://capacities-files.s3.eu-central-1.amazonaws.com/private/2f67f990-f8b4-4234-9c01-b9afa1954e3d/raw.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIA5VTNRR6EBR56K2NK%2F20260921%2Feu-central-1%2Fs3%2Faws4_request&X-Amz-Date=20260921T090944Z&X-Amz-Expires=43200&X-Amz-Signature=f2a0cb9a2d5c8e3f417b2b89065866a3368096342f94f66e550f37c9b15d936a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
[image - Notes](https://app.capacities.io/e288d27f-67f3-4518-a8cf-7caaffd5e145/2f67f990-f8b4-4234-9c01-b9afa1954e3d)

## Data Mesh

**Data is kept within several domains** within a company, such as manufacturing, sales, and suppliers. Each domain has its own mini IT team that owns its data, cleans it, creates the analytical data, and makes it available. This results in a decentralized architecture where data, people, and infrastructure are scaled out the more domains you have, the more people and infrastructure you get.

![image](https://capacities-files.s3.eu-central-1.amazonaws.com/private/7ce2fbab-7d3a-4913-8a50-8487a56c6be1/raw.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIA5VTNRR6EBR56K2NK%2F20260921%2Feu-central-1%2Fs3%2Faws4_request&X-Amz-Date=20260921T090944Z&X-Amz-Expires=43200&X-Amz-Signature=5d6f4fafb75f8a9422f7ee56fd63791b06fcfcf6d5d1c6820e767027b2c6f9ad&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
[image - Notes](https://app.capacities.io/e288d27f-67f3-4518-a8cf-7caaffd5e145/7ce2fbab-7d3a-4913-8a50-8487a56c6be1)


**Comparison of data architectures**

| RDW             | DL             | MDW                                | DF                                 | DLH            | DM             |
| :-------------- | :------------- | :--------------------------------- | :--------------------------------- | :------------- | :------------- |
| Centralized     | Centralized    | Centralized                        | Centralized                        | Centralized    | Decentralized  |
| Relational      | Object         | Relational and object              | Relational and object              | Object         | Domainspecific |
| Schema-on-write | Schema-on-read | Schema-on-read and schema-on-write | Schema-on-read and schema-on-write | Schema-on-read | Domainspecific |

# Labs

Praktikum untuk menunjukan kelemahan data semi terstruktur (JSON) yang disimpan di Relasional Datawarehouse (PostgreSQL).

`docker-compose.yaml`

```yaml
services:
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

volumes:
  pgdata:

networks:
  docker_network:
    driver: bridge
```
docker compose up -d

- Masuk ke bash container tidak lagi di host

```shell
docker exec -it postgres bash
```

- Masuk ke database postgres (default database)

```shell
psql -U admin(nama username) -d postgres
psql -U mika -d postgres --contoh
```

- Membuat tabel dengan kolom bertipe data JSON

```sql
CREATE TABLE mahasiswa_json (
    id SERIAL PRIMARY KEY,
    payload JSONB,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

- Menambahkan data ke tabel 

```sql
INSERT INTO mahasiswa_json (payload)
VALUES ('{
    "nim": "2026001",
    "nama_lengkap": "Budi Santoso",
    "program_studi": "Sistem Informasi",
    "semester": 4,
    "mata_kuliah": [
        {"kode": "SI101", "nama": "Big Data Analytics", "sks": 3, "nilai": "A"},
        {"kode": "SI102", "nama": "Data Warehouse", "sks": 3, "nilai": "B+"}
    ],
    "status_aktif": true
}');
```

- Menampilkan data JSON

```sql
SELECT 
    payload->>'nim' AS nim,
    payload->>'nama_lengkap' AS nama,
    payload->>'program_studi' AS prodi,
    payload->'mata_kuliah'->0->>'nama' AS matkul_pertama,
    payload->'mata_kuliah'->0->>'nilai' AS nilai_matkul_pertama
FROM mahasiswa_json;
```

```shell
   nim   |     nama     |      prodi       |   matkul_pertama   | nilai_matkul_pertama 
---------+--------------+------------------+--------------------+----------------------
 2026001 | Budi Santoso | Sistem Informasi | Big Data Analytics | A
```

> Evolusi dari Relasional Datawarehouse ke Data Lake mengubah cara penyimpanan data semi terstruktur (JSON) dari dalam tabel (PostgreSQL) menjadi penyimpanan *file* `.json` mentah yang lebih fleksibel.

# References

Serra, J. (2024). Deciphering data architectures. " O'Reilly Media, Inc.".



