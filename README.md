# TPC-DS com Spark 2.3

Clonando o repositório e fazendo o Build

```bash
git clone git@github.com:joao-parana/tpcds.git
cd tpcds
export TPCDS_WORKLOAD_GEN=$PWD
mvn clean package
```

Gerando os dados com Spark

```bash
/usr/local/spark/bin/spark-submit --class edu.brown.cs.systems.tpcds.spark.SparkTPCDSDataGenerator \
    ${TPCDS_WORKLOAD_GEN}/target/spark-workloadgen-4.0-jar-with-dependencies.jar
```

	
Para executar use:

```bash
/usr/local/spark/bin/spark-submit --class edu.brown.cs.systems.tpcds.spark.SparkTPCDSWorkloadGenerator \
    ${TPCDS_WORKLOAD_GEN}/target/spark-workloadgen-4.0-jar-with-dependencies.jar
```


To configure the TPC-DS data set, there are a variety of configuration options.  Most of these are inherited from Databricks spark-sql-perf, which we use to generate the TPC-DS data.

The options of interest are as follows:

 - scaleFactor specifies the dataset size.  A scale factor of n generates approximately n GB of data.  Most data formats compress this quite effectively, so on disk the data will appear smaller (eg, Parque or Orc can compress by a factor of approximately 4).
 - dataLocation specifics the location of the dataset.  Typically this will be in HDFS, and you can specify HDFS file locations as normal (eg, hdfs://<hostname>:<port>/<path>)
 - dataFormat specifies the format to store the data.  "parquet" and "orc" are good choices with high compression; "text" is also supported.

The full (default) configuration options are as follows (`src/main/resources/reference.conf`):

```java	
tpcds {
    scaleFactor = 1
    dataLocation = "hdfs://127.0.0.1:9000/tpcds"
    dataFormat = "parquet"
    overwrite = true
    partitionTables = false
    useDoubleForDecimal = false
    clusterByPartitionColumns = false
    filterOutNullPartitionValues = false
}
```

É possivel alterar o destino dos dados especificando o sistema de arquivo como mostrado abaixo.


```bash
mkdir -p /tmp/tpcds100
dataLocation = "file:///tmp/tpcds100"
```

Desta forma fica-se independente do HADOOP.


We have provided a couple of useful command line utilities, which are generated 
into the folder `target/appassembler/bin`:

* list-queries lists the available queries.  It takes zero or one arguments; with zero arguments, it lists the available benchmarks; with 1 argument, it either lists a benchmark, or prints a query.
Queries are broken down into benchmarks.
Since multiple people have implemented variants of the original TPC-DS queries, we have included multiple of these variants here.  The impala-tpcds-modified-queries are a set of 21 selected queries that several work has used for benchmarking previously with Spark.
 
* dsdgen is a wrapper around the dsdgen utility that TPC provides. This package comes with precompiled dsdgen binaries for Linux and Mac, which we use for data generation.

Exemplos de execuções de list-queries

```bash
target/appassembler/bin/list-queries
target/appassembler/bin/list-queries impala-tpcds-modified-queries
target/appassembler/bin/list-queries impala-tpcds-modified-queries/q98.sql
target/appassembler/bin/list-queries impala-tpcds-modified-queries/create-rdd.sql
```

```bash
```

```bash
```

```bash
```

