FROM postgres:11.6-alpine

ENV POSTGRES_DB=db \
    POSTGRES_USER=usr \
    POSTGRES_PASSWORD=123

COPY 01CreateScheme.sql /docker-entrypoint-initdb.d
COPY 02InsertData.sql /docker-entrypoint-initdb.d