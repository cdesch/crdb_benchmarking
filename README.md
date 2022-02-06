# CRDB Benchmarking

Benchmark CockroachDB

## Run

Start Cluster

    docker-compose -f docker-compose-crdb.yaml up

Init Cluster

    docker run --network=crdb_benchmarking_default -it cockroachdb/cockroach:latest init --insecure --host=roach1:26257

Import Data
    
    docker run --network=crdb_benchmarking_default -it cockroachdb/cockroach:latest workload fixtures import tpcc --warehouses=2500 'postgres://root@roach1:26257?sslmode=disable'

Run benchmark

    export addrs="postgres://root@roach1:26257?sslmode=disable postgres://root@roach2:26257?sslmode=disable postgres://root@roach3:26257?sslmode=disable"
    echo $addrs
    docker run --network=crdb_benchmarking_default -it cockroachdb/cockroach:latest workload run tpcc --warehouses=2500 --ramp=1m --duration=5m '$(echo $addrs)'

View CRDB Dashboard

    http://localhost:8080

## Benchmarking

[CRDB Benchmarking Reference](https://www.cockroachlabs.com/docs/v21.2/performance-benchmarking-with-tpcc-large)

