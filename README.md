# Hash Hash Hash
We built a hash table with mutexes to handle race conditions and collisions.

## Building
```shell
make clean && make
```

## Running
```shell
./hash-table-tester -t 8 -s 50000

Generation: 72,712 usec
Hash table base: 1,037,472 usec
  - 0 missing
```

## First Implementation
In the hash_table_v1_add_entry function, I implemented a single mutex to cover lines 76-119. This mutex ensures that the addition of values is treated as a critical section, thereby guaranteeing correctness by preventing two threads from simultaneously adding to the hash table.

### Performance
```shell
./hash-table-tester -t 8 -s 50000

Generation: 72,712 usec
Hash table base: 1,037,472 usec
  - 0 missing
Hash table v1: 1,435,129 usec
  - 0 missing
Hash table v2: 372,111 usec
  - 0 missing
```
Version 1 is slower than the base version due to the overhead associated with creating, locking, and unlocking threads, which leads to slightly slower execution. By treating the addition of values as a critical section, parallelism is eliminated, further impacting performance.

## Second Implementation
In the `hash_table_v2_add_entry` function, I added one mutex per hash table entry to ensure both performance and correctness. These mutexes block the addition to each specific entry, allowing other buckets to still accept entries and thus enabling parallelism. This setup prevents two threads from simultaneously inserting at the head or inserting duplicate keys, ensuring correctness. I placed the locks around the critical sections.

### Performance
```shell
./hash-table-tester -t 8 -s 50000
```

TODO more results, speedup measurement, and analysis on v2
The speedup is approximately (how many times faster)x, which is comparable to the number of physical cores (how many cores). This speedup is due to (which sections contribute to parallelization).

## Cleaning up
```shell
make clean
```