# Install RocksDB Action

Builds RocksDB v10.5.1 as a shared library on Ubuntu runners and caches the result so subsequent jobs can reuse the compiled artifacts. The action installs the headers, shared objects, and pkg-config metadata into `/usr/local`.

## What it does

- Restores a cache that stores the compiled RocksDB shared libraries, headers, and pkg-config files.
- Installs the system dependencies required to build RocksDB when the cache is cold.
- Clones facebook/rocksdb at tag `v10.5.1`, builds `make shared_lib` with `PORTABLE=1`, and runs `make install-shared`.
- Runs `ldconfig` to ensure the linker is aware of the new shared objects.

### Usage

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: initia-labs/install-rocksdb@v1
```

### Cached files

The cache key is `rocksdb-v10.5.1-${{ runner.os }}` and it stores:

```plaintext
/usr/local/lib/librocksdb.so.10.5.1
/usr/local/lib/librocksdb.so.10.5
/usr/local/lib/librocksdb.so.10
/usr/local/lib/librocksdb.so
/usr/local/include/rocksdb
/usr/local/lib/pkgconfig/rocksdb.pc
```

### Notes

- The action assumes `sudo` access, which is available on the default GitHub-hosted Ubuntu runners.
- For other operating systems or RocksDB versions, fork the action and adjust `action.yml` accordingly.
