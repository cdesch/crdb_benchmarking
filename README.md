# CRDB Benchmarking

Benchmark CockroachDB

## Run

    docker-compose -f docker-compose-crdb.yaml up
    docker run --network=crdb_benchmarking_default -it cockroachdb/cockroach:latest init --insecure --host=roach1:26257

    docker run --network=crdb_benchmarking_default -it cockroachdb/cockroach:latest cockroach workload fixtures import tpcc --warehouses=2500 'postgres://root@roach1:26257?sslmode=disable'
    
    docker run --network=crdb_benchmarking_default -it cockroachdb/cockroach:latest workload fixtures import tpcc --warehouses=2500 'postgres://root@roach1:26257?sslmode=disable'

Dashboard

    http://localhost:8080

## Benchmarking

[CRDB Benchmarking Reference](https://www.cockroachlabs.com/docs/v21.2/performance-benchmarking-with-tpcc-large)

