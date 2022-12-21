# How To Generate Data With Pagila in Percona Distribution for PostgreSQL

[Percona Distribution for PostgreSQL](https://www.percona.com/software/postgresql-distribution) is a collection of tools to assist you in managing your PostgreSQL database system: it installs PostgreSQL and complements it by a selection of extensions that enable solving essential practical tasks efficiently.

For this guide, we will use [Pagila](https://github.com/devrimgunduz/pagila), a tool that provides a standard schema that we can use for examples in books, tutorials, articles, samples, etc. The project is open source; you can clone it and start to run the queries. With Pagila, we will create all schema objects and insert the data into our tables.

Let's start it!

## Requirements

- [Docker](https://docs.docker.com/engine/install/ubuntu/)

## Installing Percona Distribution for PostgreSQL

1. Pull the Percona Distribution for PostgreSQL image. Version 12.13.

```bash
docker pull perconalab/percona-distribution-postgresql:12.13
```

2.  Run Percona Distribution PostgreSQL container. You must specify POSTGRES_PASSWORD as a non-empty value for the superuser.

```bash
docker run --name percona-postgres -e POSTGRES_PASSWORD=secret -d perconalab/percona-distribution-postgresql:12.13
```

## Using Pagila to Generate Data

Clone the Pagila GitHub repository

```bash
git clone https://github.com/devrimgunduz/pagila
```

Enter in the Percona Distribution PostgreSQL container

```bash
docker exec -it  percona-postgres psql -U postgres
```

Create a database called perconadb

```bash
CREATE DATABASE perconadb;
\q
```

Create all schema objects (tables, etc.)

```bash
# In Pagila directory
cd pagila

# execute the script pagila-schema.sql i
cat pagila-schema.sql | docker exec -i percona-postgres psql -U postgres -d perconadb
```

Insert all data

```bash
cat pagila-data.sql | docker exec -i percona-postgres psql -U postgres -d perconadb
```

Validate the data; let’s check if the tables were created and if it is populated with data

```bash
docker exec -it percona-postgres psql -U postgres
\c perconadb
\dt
```

```mysql
SELECT * FROM inventory
```

You are ready! The data is there and ready to be used.

You can refer to the official documentation of [Percona Distribution for PostgreSQL](https://www.percona.com/software/postgresql-distribution) if you want to know the entire collection of tools to help you manage your PostgreSQL database system.
You can check [Pagila](https://github.com/devrimgunduz/pagila) open source project to generate data for Postgres.
