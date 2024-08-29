# Cylclist Bike Share Behavior in Chicago 🚲

## Setup environment

```
- python -m venv my_env
- source my_env\Scripts\activate || my_env\Scripts\activate
- pip install packages || requirements.txt
```

### Basic Packages
1. pandas
2. scipy
3. matplotlib
4. seaborn
5. python-dotenv
6. requests

### Large Files
1. dask[complete]:
```
1. Core Dask Components:
    dask: The main Dask package for parallel computing.
    distributed: For running Dask on a cluster (multi-node or multi-core).

2. Array and DataFrame Support:
    numpy: For array operations.
    pandas: For DataFrame operations.
    zarr: For chunked, compressed, N-dimensional arrays.
    h5py: For HDF5 file support.

3. Machine Learning and Data Processing:
    scikit-learn: For machine learning tasks.
    joblib: For parallel computing tasks, often used with scikit-learn.
    pyarrow: For Arrow in-memory format, often used in data processing.

4. Visualization:
    bokeh: For interactive visualizations.
    matplotlib: For static plots.
    holoviews: For building complex visualizations easily.
    datashader: For large-scale data visualization.

5. Data I/O:
    fastparquet: For Parquet file support.
    s3fs: For working with Amazon S3.
    gcsfs: For working with Google Cloud Storage.
    fsspec: For file system specification (for accessing different file systems).
    dask-sql: For SQL-like queries on Dask DataFrames.

6. Additional Dependencies:
    toolz: For functional utilities.
    cloudpickle: For serializing Python objects.
    msgpack: For message serialization.
    pint: For units of measurement.
```
2. dask-cuda: 
```
1. Core Dask and CUDA Integration:
    dask-cuda: The main package that integrates Dask with CUDA for GPU-based parallel computing.
    dask: The core Dask package for parallel computing (if not already installed).
    distributed: For running Dask on a cluster, used in conjunction with GPUs.

2. CUDA Libraries:
    cudf: A GPU DataFrame library similar to Pandas but built on Apache Arrow and NVIDIA RAPIDS. It provides high-performance data manipulation and processing on GPUs.
    cuml: A machine learning library designed for GPUs, providing algorithms similar to scikit-learn.
    rmm: RAPIDS Memory Manager for managing GPU memory efficiently.

3. Additional Dependencies:
    numba: A just-in-time (JIT) compiler for Python that allows you to write CUDA kernels directly in Python.
    cupy: A NumPy-like library for GPU arrays, providing many of the same functions but optimized for GPUs.
    nvtx: NVIDIA Tools Extension for annotating code and measuring performance.
    ucx-py: Python bindings for the UCX communication library, which allows for high-performance communication between GPUs and CPUs.
    dask-cudf: Integration of Dask with cuDF, enabling distributed DataFrame operations on GPUs.
```

### Modeling Packages
1. statsmodels 
2. scikit-learn

## Install Git lfs

```
- git lfs install
- git lfs track 'file_name.extension'
```

## Get Data from Git lfs

```
- git lfs pull
```

## Detailed Report
o) Detailed Report can be seen in 
