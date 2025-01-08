# HOW TO USE DASK ON CONDA ENV

## Installation
```bash
conda install dask distributed -c conda-forge
conda install cloudpickle toolz tornado -c conda-forge
```

## Penyesuaian Berdasarkan Dataset
Jika Anda tahu ukuran dataset Anda, berikut adalah panduan alokasi memori:

| Ukuran Dataset       | Memory Limit (per Worker) | Jumlah Worker yang Disarankan |
|-----------------------|---------------------------|--------------------------------|
| < 500 MB             | 512 MB - 1 GB            | 2 - 4 Workers                 |
| 500 MB - 2 GB        | 1 GB - 2 GB              | 2 - 3 Workers                 |
| 2 GB - 5 GB          | 2 GB - 4 GB              | 2 Workers                     |
| > 5 GB               | > 4 GB                   | 1 - 2 Workers                 |

## TERMINAL
```bash
dask scheduler --host localhost
dask worker --nanny tcp://localhost:8786 --nworkers 2 --nthreads 2 --memory-limit 3GB
```

## Using Subprocess
```python
from subprocess import Popen

# Jalankan scheduler
scheduler = Popen(["dask-scheduler"])

# Jalankan multiple worker
worker1 = Popen(["dask-worker", "tcp://127.0.0.1:8786", "--nthreads", "2"])
worker2 = Popen(["dask-worker", "tcp://127.0.0.1:8786", "--nthreads", "2"])
```

TIPS
1. bagaimana cara menggunakan Chunk yang Tepat dimana ukuran chunk (partisi) sesuai dengan kapasitas memori
2. bagaimana cara menggunakan Scheduler yang Tepat Gunakan scheduler "threads", "processes", "distributed", dll sesuai kebutuhan

1. Setiap chunk sebaiknya berukuran 1% hingga 10% dari total memori yang tersedia per worker, misal Jika memori worker adalah 8 GB, gunakan chunk berukuran 80 MB hingga 800 MB.

    1. Gunakan sample() untuk Mengestimasi Ukuran Chunk
    ```python
    # Sampel data untuk menghitung ukuran rata-rata per baris
    sample = dd.read_csv('large_file.csv', sample=1e6)  # Ambil 1 juta baris pertama
    memory_per_row = sample.memory_usage(deep=True).sum() / len(sample)

    # Estimasi ukuran chunk (misalnya 200 MB)
    rows_per_chunk = 200 * 1024**2 / memory_per_row
    print(f"Ukuran chunk optimal adalah sekitar {rows_per_chunk:.0f} baris.")
    ```

2. Penggunaan scheduler
# Faktor-Faktor yang Perlu Dipertimbangkan
## Kompleksitas Komputasi
1. Jika komputasi I/O-bound (misalnya, membaca file atau operasi disk), gunakan scheduler threads.
2. Jika komputasi CPU-bound (misalnya, agregasi, perhitungan matematis), gunakan scheduler processes.

## Jumlah Core CPU
1. Jika Anda memiliki CPU dengan banyak core (misalnya, 8 core atau lebih), processes dapat memanfaatkan semuanya.
2. Jika CPU terbatas (misalnya, hanya 2-4 core), threads mungkin cukup.

## Kapasitas Memori
1. Jika dataset mendekati kapasitas memori sistem Anda, pertimbangkan penggunaan chunk (partisi) dengan Dask dan gunakan Distributed Scheduler untuk lebih fleksibel.


3. Rekomendasi Scheduler
## I/O-Bound Komputasi (misalnya, membaca file besar atau penggabungan data):
1. Gunakan scheduler: threads.

## CPU-Bound Komputasi (misalnya, agregasi, operasi matematis berat):
1. Gunakan scheduler: processes.

## Distribusi Beban Lebih Baik atau Banyak Task Paralel:
1. Gunakan scheduler: distributed.


| **Komputasi**                     | **Jumlah Core CPU**   | **Scheduler yang Direkomendasikan** |
|-----------------------------------|-----------------------|-------------------------------------|
| I/O-Bound (Baca/Tulis File)       | Tidak penting         | threads                             |
| CPU-Bound (Agregasi Berat)        | < 4 core              | threads                             |
| CPU-Bound (Agregasi Berat)        | >= 4 core             | processes                           |
| Banyak Tugas Paralel atau Cluster | Multi-node/Cluster    | distributed                         |

# Optimasi Dask
1. Garbage collection
gunakan setelah melakukan compute() atau persist()

2. Persist()
gunakan ketika data digunakan secara berulang, menerapkan filter, atau map_partitions

# Rekomendasi Umum untuk Max Blocksize:
Gunakan panduan ini:

RAM 8 GB → Max blocksize: 128 MB - 256 MB.
RAM 16 GB → Max blocksize: 256 MB - 512 MB.
RAM 32 GB atau lebih → Max blocksize: 512 MB - 1 GB.

# dask spill to disk
pip install diskcache

```python
from dask.cache import Cache
import dask

# Tentukan direktori cache
cache = Cache('/path/to/directory')  # Ganti '/path/to/directory' dengan lokasi direktori untuk cache
cache.register()

# Contoh array Dask
import dask.array as da
x = da.random.random((10000, 10000), chunks=(1000, 1000))

# Operasi akan menggunakan cache jika memori penuh
y = x + 1
result = y.compute()

```


# Rekomendasi Waktu sleep untuk Dataset Anda
Jika ingin menambahkan jeda tetap:

1. Partisi besar (1 juta+ baris): 1-2 detik.
2. Partisi sedang (500k-1 juta baris): 0.5-1 detik.
3. Partisi kecil (<500k baris): 0.1-0.5 detik.

len vs shape[0]
len for each partition - shape all rows in dataframe