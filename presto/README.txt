Presto is a distributed SQL query engine.

Please see the website for installation instructions:

https://prestodb.github.io/


Installed used this configuration
https://prestodb.github.io/docs/current/installation/deployment.html#installing-presto


bin/launcher run
bin/launcher start
bin/launcher stop

// build the docker server 
docker build --build-arg PRESTO_VERSION=0.284 . -t prestodb:latest
// start the docker server 
docker run --name presto -p 8080:8080 prestodb:latest  

To connect using cli - second presto is presto.jar file and firs is docker name
docker exec -it presto presto


// Sample query

SELECT
       l.returnflag,
        l.linestatus,
        sum(l.quantity)                                       AS sum_qty,
        sum(l.extendedprice)                                  AS sum_base_price,
        sum(l.extendedprice * (1 - l.discount))               AS sum_disc_price,
        sum(l.extendedprice * (1 - l.discount) * (1 + l.tax)) AS sum_charge,
        avg(l.quantity)                                       AS avg_qty,
        avg(l.extendedprice)                                  AS avg_price,
        avg(l.discount)                                       AS avg_disc,
        count(*)                                              AS count_order
      FROM
        tpch.sf1.lineitem AS l
      WHERE
        l.shipdate <= DATE '1998-12-01' - INTERVAL '90' DAY
      GROUP BY
        l.returnflag,
        l.linestatus
      ORDER BY
        l.returnflag,
        l.linestatus;


https://prestodb.io/docs/current/installation/jdbc.html

https://prestodb.io/docs/current/sql/show-columns.html

SHOW SCHEMAS FROM postgresql;
SHOW TABLES FROM postgresql.public;
DESCRIBE postgresql.public.document;
SHOW COLUMNS FROM postgresql.public.document;
select name, description from postgresql.public.document;


-------
MongoDB setup 

https://www.mongodb.com/docs/manual/tutorial/install-mongodb-community-with-docker/

docker container ls

-- Open the port
docker run --name mongo -p 27017:27017 -d mongodb/mongodb-community-server:latest

-- Connect to container
docker exec -it fincustomer mongosh 
db.runCommand(
   {
      hello: 1
   }
)
 use admin
 db.createUser(
   {
     user: "prem",
     pwd: "Welcome2023", // or cleartext password
     roles: [ 
        { role: "userAdminAnyDatabase", db: "admin" }, 
             { role: "dbAdminAnyDatabase", db: "admin" }, 
             { role: "readWriteAnyDatabase", db: "admin" } 
     ]
   }
 )
 mongodb://localhost:27017/fincustomer
 mongodb://username:password@localhost:27017/?authSource=admin 

 -----

 SELECT * FROM mongodb.bank.customers WHERE name = 'Leslie Martinez'

SELECT c.name, a.products
FROM mongodb.bank.customers c
CROSS JOIN UNNEST(c.accounts) AS t(account_id)
JOIN mongodb.bank.accounts a ON t.account_id = a.account_id
WHERE c.name = 'Leslie Martinez'

SELECT table_catalog, 
        table_schema,
        table_name,
        column_name,
        data_type,
        is_nullable
    FROM
        {catalog}.information_schema.columns
    WHERE table_schema != 'information_schema' or table_schema != 'pg_catalog'

show catalogs