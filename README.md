# Getting Started with Flink SQL and Apache Iceberg

The full story is published on [Medium blog](https://lazypro.medium.com/71b96817e3c3).

This repo was inspired by [this article](https://www.dremio.com/blog/getting-started-with-flink-sql-and-apache-iceberg/) and fixes many fatal bugs.

The entire playground is completely free, and at its core are the following three components.
1. DynamoDB: This is the catalog store, and uses the local version to avoid expenses.
2. Minio: This is where the actual iceberg is stored, again using minio to avoid the expense of s3.
3. Flink: the key to Playground.

Because all the URLs of the dependency packages are written in the `sql-client` self-contained image, if you run into a situation where you can't build an image, you have to change the corresponding path.

Alternatively, you can use an already created docker image.
> docker pull wirelessr/flink-iceberg:1.16.3

## Playing Steps

0. Build the container: `docker compose build sql-client`
1. Directly activate the entire environment: `docker compose up -d`.
2. Launch the SQL Client `docker compose exec sql-client bash -c "./bin/sql-client.sh"`
4. Create and use the DynamoDB Catalog.

```sql
CREATE CATALOG dynamo_catalog WITH (
'type' = 'iceberg',
'catalog-impl' = 'org.apache.iceberg.aws.dynamodb.DynamoDbCatalog',
'io-impl' = 'org.apache.iceberg.aws.s3.S3FileIO',
'client.assume-role.region' = 'us-east-1',
'warehouse' = 's3://warehouse',
's3.endpoint' = 'http://storage:9000',
's3.path-style-access' = 'true',
'dynamodb.table-name' = 'iceberg-catalog',
'dynamodb.endpoint' = 'http://dynamodb-local:8000');

USE CATALOG dynamo_catalog;
```

5. Create the table.

```sql
CREATE database db;
USE db;

CREATE TABLE spotify (songid BIGINT, artist STRING, rating BIGINT);
```

6. Insert and read data.

```sql
INSERT INTO spotify VALUES (2, 'drake', 3);
SELECT * FROM spotify;
```

## Viewing the Iceberg data in MinIO

1. Go to http://localhost:9001/ and login (`admin` / `password`)
2. Inspect the `warehouse` bucket, observe the Parquet files under `data` and JSON files under `metadata`

## Viewing the catalog data in DynamoDB

1. Install [dynamodb-admin](https://github.com/aaronshaf/dynamodb-admin): `npm install -g dynamodb-admin`
2. Launch GUI `DYNAMO_ENDPOINT=http://localhost:8000 dynamodb-admin`
3. Go to http://0.0.0.0:8001/ and inspect the values held under `iceberg-catalog`
