---
type: 'BigData'
collections: '20261'
title: '#6 Data Type Classification'
createdAt: '2026-08-05T03:30:20.415Z'
lastUpdated: '2026-10-07T02:28:41.894Z'
tags: []
url: null
---

# #6 Data Type Classification

# Semi-Structured

## Human-Readable

### JSON

JSON (JavaScript Object Notation) adalah format pertukaran data berbasis teks yang ringan (*lightweight*), mudah dibaca oleh manusia (*human-readable*), dan mudah diproses oleh mesin.

---

Object

```json
{
  "nim": "20261001",
  "nama": "Rian Pratama",
  "ipk": 3.85
}
mhs['nama']
mhs->nama
```


---

Array

```text
[
  "Apache Spark",
  "ClickHouse",
  "DuckDB",
  "PostgreSQL"
]
database[2]
```


---

---

Nested Object

```json
{
  "id_dosen": "D104",
  "nama": "Dr. Aris",
  "profil_akademik": {
    "jabatan": "Lektor Kepala",
    "bidang_keahlian": "Sistem Terdistribusi",
    "laboratorium": {
      "nama_lab": "Lab Komputasi Paralel",
      "gedung": "Gedung Utama Lt. 3"
    }
  }
}
profil_akademik['laboratorium']['nama_lab']
profil_akademik.laboratorium.nama_lab
profil_akademik->laboratorium->nama_lab
```


---

Nested Array

```text
angka = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
]

angka[2][2]
```


---

Array of Objects

```json
{
  "kode_kelas": "BD-2026",
  "matakuliah": "Data Warehouse",
  "mahasiswa": [
    {
      "nim": "2026001",
      "nama": "Budi",
      "nilai_harian": [85, 90, 88]
    },
    {
      "nim": "2026002",
      "nama": "Siti",
      "nilai_harian": [92, 95, 90]
    }
  ]
}
```

JSON Schema

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "User",
  "type": "object",
  "properties": {
    "id": {
      "type": "integer"
    },
    "name": {
      "type": "string"
    },
    "email": {
      "type": "string",
      "format": "email"
    },
    "age": {
      "type": "integer",
      "minimum": 18
    }
  },
  "required": ["id", "name", "email"]
}
```

### XML

XML (Extensible Markup Language) adalah bahasa markah berbasis teks yang dirancang untuk menyimpan dan mentransfer data secara terstruktur. Berbeda dari HTML yang berfokus pada cara menampilkan data di layar, XML berfokus pada struktur dan makna data melalui *tag* yang dapat didefinisikan sendiri.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<perpustakaan>
  <buku id="B001" kategori="Komputer">
    <judul>Konsep Sistem Terdistribusi</judul>
    <penulis>
      <nama_depan>Aris</nama_depan>
      <nama_belakang>Pratama</nama_belakang>
    </penulis>
    <tahun_terbit>2026</tahun_terbit>
    <harga mata_uang="IDR">125000</harga>
  </buku>
</perpustakaan>
```

XML Schema

XML Schema Definition (XSD) adalah dokumen terpisah yang digunakan untuk mendefinisikan aturan, struktur, batasan (*constraints*), dan tipe data (seperti `xs:string`, `xs:integer`, `xs:date`) dari sebuah dokumen XML.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="perpustakaan">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="buku" type="bukuType" maxOccurs="unbounded"/>
      </xs:sequence>
    </xs:complexType>
  </xs:element>
  <xs:complexType name="bukuType">
    <xs:sequence>
      <xs:element name="judul" type="xs:string"/>
      <xs:element name="penulis" type="penulisType"/>
      <xs:element name="tahun_terbit" type="xs:integer"/>
      <xs:element name="harga" type="hargaType"/>
    </xs:sequence>
    <xs:attribute name="id" type="xs:string" use="required"/>
    <xs:attribute name="kategori" type="xs:string"/>
  </xs:complexType>
  <xs:complexType name="penulisType">
    <xs:sequence>
      <xs:element name="nama_depan" type="xs:string"/>
      <xs:element name="nama_belakang" type="xs:string"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="hargaType">
    <xs:simpleContent>
      <xs:extension base="xs:decimal">
        <xs:attribute name="mata_uang" type="xs:string" use="required"/>
      </xs:extension>
    </xs:simpleContent>
  </xs:complexType>
</xs:schema>
```

## Binary / Machine-Oriented Formats

| **id** | **tanggal** | **produk** | **jumlah** |
| :----- | :---------- | :--------- | :--------- |
| 1      | 2026-10-01  | Laptop     | 15000000   |
| 2      | 2026-10-01  | Mouse      | 250000     |
| 3      | 2026-10-02  | Keyboard   | 750000     |

**Row-oriented**

```text
[1, 2026-10-01, Laptop, 15000000],
[2, 2026-10-01, Mouse, 250000],
[3, 2026-10-02, Keyboard, 750000]
```

**Column-oriented**

```text
Blok id: [1, 2, 3]
Blok tanggal: [2026-10-01, 2026-10-01, 2026-10-02]
Blok produk: ['Laptop', 'Mouse', 'Keyboard']
Blok jumlah: [15000000, 250000, 750000]
```

---

```sql
SELECT SUM(Jumlah) FROM penjualan;
```

```sql
SELECT SUM(Jumlah) FROM penjualan where jumlah > 10000000;
```


---

- **Row-Oriented:** Engine harus membaca **seluruh blok data** (termasuk id, tanggal, dan produk) ke RAM, lalu memfilter/mengambil nilai `Jumlah`.

- **Column-Oriented:** Engine hanya membaca **blok disk untuk kolom** `jumlah`. Kolom `id`, `tanggal`, dan `produk` sama sekali tidak dibaca dari disk, sehingga mengurangi I/O hingga puluhan kali lipat.


---

---

### Apache Avro

- Row-oriented

- Schema

- Schema evolution

- Serialization / Deserialization

- Penggunaan pada data pipeline dan Kafka


---

### Apache Parquet

- Column-oriented

- Columnar storage

- Compression

- Predicate pushdown

- Cocok untuk analytical workload


---

### Apache ORC

- Column-oriented

- Compression

- Predicate pushdown

- Cocok untuk analytical processing


---

# Unstructured

---

## Text

Plain Text: TXT, Log, Dokumen teks

Documents: PDF, DOC/DOCX, Email

![image](https://capacities-files.s3.eu-central-1.amazonaws.com/private/108d43b6-4b61-48fd-a5e7-a4605179ac35/raw.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIA5VTNRR6EBR56K2NK%2F20261007%2Feu-central-1%2Fs3%2Faws4_request&X-Amz-Date=20261007T021256Z&X-Amz-Expires=43200&X-Amz-Signature=89e34be0f5e8c06f523cf72caecdea3fd31cecee218ab2fe559f44801551cfe3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
[image - Notes](https://app.capacities.io/e288d27f-67f3-4518-a8cf-7caaffd5e145/108d43b6-4b61-48fd-a5e7-a4605179ac35)

---

## Image

JPG/JPEG, PNG, TIFF

![struk](https://capacities-files.s3.eu-central-1.amazonaws.com/private/9be942b9-6e5b-4594-af68-808a860869fe/raw.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIA5VTNRR6EBR56K2NK%2F20261007%2Feu-central-1%2Fs3%2Faws4_request&X-Amz-Date=20261007T021212Z&X-Amz-Expires=43200&X-Amz-Signature=53b5d8f4cb416a94bd62feed34cca921772a3fee3aee8c03ecf1c5987f5c0965&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
[struk - Notes](https://app.capacities.io/e288d27f-67f3-4518-a8cf-7caaffd5e145/9be942b9-6e5b-4594-af68-808a860869fe)

---

## Audio

MP3, WAV, FLAC

![image](https://capacities-files.s3.eu-central-1.amazonaws.com/private/29b1fe16-7bdc-422d-9727-8c64b5d75aea/raw.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIA5VTNRR6EBR56K2NK%2F20261007%2Feu-central-1%2Fs3%2Faws4_request&X-Amz-Date=20261007T021330Z&X-Amz-Expires=43200&X-Amz-Signature=85f644a5cae07b65802e8ec908b4862ab4488fc4c0e0c36538b76f6b1feb5c8c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
[image - Notes](https://app.capacities.io/e288d27f-67f3-4518-a8cf-7caaffd5e145/29b1fe16-7bdc-422d-9727-8c64b5d75aea)

---

## Video

MP4, AVI, MKV

![image](https://capacities-files.s3.eu-central-1.amazonaws.com/private/fa671f22-87f6-4169-be42-df7be60a0b2c/raw.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIA5VTNRR6EBR56K2NK%2F20261007%2Feu-central-1%2Fs3%2Faws4_request&X-Amz-Date=20261007T021401Z&X-Amz-Expires=43200&X-Amz-Signature=ca786d9a7f8e433c50bf86f5b3a7f9f66ab1886d1df781879e830fa8a5f7a2de&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
[image - Notes](https://app.capacities.io/e288d27f-67f3-4518-a8cf-7caaffd5e145/fa671f22-87f6-4169-be42-df7be60a0b2c)

---

# Labs

Dataset download disini [transactions.csv](https://drive.google.com/file/d/1UmBbvYiiGAzsa9lf3GFXWPKnCcout7az/view?usp=sharing)

```python
# !pip install pandas pyarrow fastavro
```

```python
import pandas as pd
import pyarrow as pa
import pyarrow.orc as orc
import fastavro

df = pd.read_csv("transactions.csv")
df
```

Write Avro

```python
records = df.to_dict(orient="records")
fields = []
for col, dtype in df.dtypes.items():
    if "int" in str(dtype):
        avro_type = ["null", "long"]
    elif "float" in str(dtype):
        avro_type = ["null", "double"]
    elif "bool" in str(dtype):
        avro_type = ["null", "boolean"]
    else:
        avro_type = ["null", "string"]
    fields.append({"name": col, "type": avro_type})
schema = {
    "doc": "Transactions Data",
    "name": "Transaction",
    "type": "record",
    "fields": fields
}
parsed_schema = fastavro.schema.parse_schema(schema)
with open("transactions.avro", "wb") as out:
    fastavro.writer(out, parsed_schema, records)
```

Read Avro

```python
with open("transactions.avro", "rb") as fp:
    reader = fastavro.reader(fp)
    records = list(reader)
avro = pd.DataFrame(records)
avro
```

ORC

```python
table = pa.Table.from_pandas(
    df,
    preserve_index=False
)
orc.write_table(
    table,
    "transactions.orc",
    compression="zstd"
)
orc = pd.read_orc('transactions.orc')
orc
```

Parquet

```python
#konversi dataframe to parquet
df.to_parquet(
    "transactions.parquet",
    engine="pyarrow"
)
# read parquet file
parquet = pd.read_parquet('transactions.parquet', engine='pyarrow')
parquet
```

**Unstractured**

gunakan gambar berikut 

![struk](https://capacities-files.s3.eu-central-1.amazonaws.com/private/9be942b9-6e5b-4594-af68-808a860869fe/raw.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIA5VTNRR6EBR56K2NK%2F20261007%2Feu-central-1%2Fs3%2Faws4_request&X-Amz-Date=20261007T021212Z&X-Amz-Expires=43200&X-Amz-Signature=53b5d8f4cb416a94bd62feed34cca921772a3fee3aee8c03ecf1c5987f5c0965&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
[struk - Notes](https://app.capacities.io/e288d27f-67f3-4518-a8cf-7caaffd5e145/9be942b9-6e5b-4594-af68-808a860869fe)

jalankan jupyter

```bash
docker exec -u 0 -it jupyter sh -c "apt-get update && apt-get install -y tesseract-ocr tesseract-ocr-ind libgl1"
```

install library

```python
!pip install pytesseract opencv-python-headless
```

```python
import cv2
import pytesseract
import re
import pandas as pd

def extract_receipt_items(image_path):
    img = cv2.imread(image_path)
    config = '--psm 6'
    text_content = pytesseract.image_to_string(img, lang='ind+eng', config=config)
    
    lines = [line.strip() for line in text_content.split('\n') if line.strip()]
    items = []
    
    i = 0
    while i < len(lines):
        match_title = re.match(r'^\d+\.\s*(.+)', lines[i])
        if match_title:
            nama_produk = match_title.group(1).strip()
            
            if not nama_produk.replace('.', '').strip():
                i += 1
                continue
            
            detail_lines = []
            j = i + 1
            while j < len(lines) and not re.match(r'^\d+\.', lines[j]) and not re.search(r'Total', lines[j], re.IGNORECASE):
                detail_lines.append(lines[j])
                j += 1
            
            detail_text = " ".join(detail_lines)
            
            qty = None
            match_qty = re.search(r'^\s*(.*?)\s*x', detail_text, re.IGNORECASE)
            if match_qty:
                qty = match_qty.group(1).strip()
            
            harga = None
            match_rp = re.search(r'(?:Rp|RP|rp|Pp|Bp|P)\s*([\d\.,\s]+)', detail_text, re.IGNORECASE)
            if match_rp:
                digits = re.sub(r'\D', '', match_rp.group(1))
                if digits:
                    harga = int(digits)
            if harga is None:
                all_numbers = re.findall(r'[\d\.,]+', detail_text)
                if all_numbers:
                    last_num_clean = re.sub(r'\D', '', all_numbers[-1])
                    if last_num_clean:
                        harga = int(last_num_clean)
            items.append({
                "nama_produk": nama_produk,
                "qty": qty,
                "harga": harga
            })
            i = j - 1
        i += 1
    return pd.DataFrame(items)

df = extract_receipt_items('struk.png')
df = df[df['harga'] > 100].reset_index(drop=True)

print(df)
```

**output**

```shell
	nama_produk	qty	harga
0	Indomie Goreng	1 lusin	36000
1	Fruit Tea Apple	1 500 ml	7000
2	Belfood Sosis Bakar	1	27000

```

# References

