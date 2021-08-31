# Overview

The project generates a fake 1 million json records and dumps them into a table where we create different types of indexes to perform various search/filter operations on the data.

# Pre-requisites

1. Postgres 12+
2. Docker
3. psql 
4. Python 3.x

# Commands

## Run postgres

`docker-compose up`

## Create table

`create table authors (data jsonb);`

## Generate 1 million records

`python generator.py`

## Populate table

`cat data.json | psql -h localhost -p 5432 test -U test -c "COPY authors (data) FROM STDIN;"`

## Create Generalized Inverted Index

`CREATE INDEX idx_authors_gin ON authors USING GIN (data);`

## Verify index use

`SELECT * FROM authors where data @> '{"author":"Andrew Proctor"}'`

## Check index size

`SELECT pg_size_pretty (pg_indexes_size('authors'));`
