# Install RocksDB Action

Builds RocksDB v10.5.1 as a shared library on Ubuntu runners and caches the result so subsequent jobs can reuse the compiled artifacts. The action installs the headers, shared objects, and pkg-config metadata into `/usr/local`.

## What it does

- Restores a cache that stores the compiled RocksDB shared libraries, headers, and pkg-config files.
- Installs the system dependencies required to build RocksDB when the cache is cold.
- Clones facebook/rocksdb at tag `v10.5.1`, builds `make shared_lib` with `PORTABLE=1`, and runs `make install-shared`.
- Syncs the cached build artifacts into `/usr/local` so compilers can find `rocksdb/c.h` and the shared objects.
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

The cache key is `rocksdb-v10.5.1-${{ runner.os }}` and it stores a copy of the installation tree at `$HOME/.rocksdb`, which the action then copies into `/usr/local`:

```plaintext
$HOME/.rocksdb/lib/librocksdb.so.10.5.1
$HOME/.rocksdb/lib/librocksdb.so.10.5
$HOME/.rocksdb/lib/librocksdb.so.10
$HOME/.rocksdb/lib/librocksdb.so
$HOME/.rocksdb/include/rocksdb
$HOME/.rocksdb/lib/pkgconfig/rocksdb.pc
```

### Notes

- The action assumes `sudo` access, which is available on the default GitHub-hosted Ubuntu runners.
- For other operating systems or RocksDB versions, fork the action and adjust `action.yml` accordingly.
