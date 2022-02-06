# CRDB Benchmarking

Benchmark CockroachDB

## Run

Start Cluster

    docker-compose -f docker-compose-crdb.yaml up

Init Cluster

    docker run --network=crdb_benchmarking_default -it cockroachdb/cockroach:latest init --insecure --host=roach1:26257

Run benchmark
    
    docker run --network=crdb_benchmarking_default -it cockroachdb/cockroach:latest workload fixtures import tpcc --warehouses=2500 'postgres://root@roach1:26257?sslmode=disable'

View CRDB Dashboard

    http://localhost:8080

## Benchmarking

[CRDB Benchmarking Reference](https://www.cockroachlabs.com/docs/v21.2/performance-benchmarking-with-tpcc-large)

