# CRDB Benchmarking

Benchmark CockroachDB with Docker Compose

## Benchmarking

[CRDB Benchmarking Reference](https://www.cockroachlabs.com/docs/v21.2/performance-benchmarking-with-tpcc-large)

## Run

Start Cluster

    docker-compose -f docker-compose-crdb.yaml up

Init Cluster if not Initialized

    docker run --network=crdb_benchmarking_default -it cockroachdb/cockroach:latest init --insecure --host=roach1:26257

View CRDB Dashboard

    http://localhost:8080

Import Benchmarking Database Data
    
    docker run --network=crdb_benchmarking_default -it cockroachdb/cockroach:latest workload fixtures import tpcc --warehouses=500 'postgres://root@roach1:26257?sslmode=disable'
    docker run --network=crdb_benchmarking_default -it cockroachdb/cockroach:latest workload fixtures import tpcc --warehouses=1000 'postgres://root@roach1:26257?sslmode=disable'

    docker run --network=crdb_benchmarking_default -it cockroachdb/cockroach:latest workload fixtures import tpcc --warehouses=2500 'postgres://root@roach1:26257?sslmode=disable'

Run benchmark

    export addrs="postgres://root@roach1:26257?sslmode=disable postgres://root@roach2:26257?sslmode=disable postgres://root@roach3:26257?sslmode=disable"
    echo $addrs
    docker run --network=crdb_benchmarking_default -it cockroachdb/cockroach:latest workload run tpcc --warehouses=2500 --ramp=1m --duration=5m '$(echo $addrs)'
    
    docker run --network=crdb_benchmarking_default -it cockroachdb/cockroach:latest workload run tpcc --warehouses=500 --ramp=1m --duration=5m 'postgres://root@roach1:26257?sslmode=disable' 'postgres://root@roach2:26257?sslmode=disable' 'postgres://root@roach3:26257?sslmode=disable'

Delete Data - Attach  to `crdb_roach1` container and CRDB SQL CLI to drop database
    
    docker exec -it crdb_roach1 ./cockroach sql --insecure
    DROP database tpcc CASCADE;

Moniter Docker Usage
    
    docker stats --format "table {{.Name}}\t{{.CPUPerc}}\t{{.MemUsage}}"
