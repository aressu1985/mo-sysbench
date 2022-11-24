# mo-sysbench
This project is customed from sysbench for MySQL. And this project just contains oltp test suits, and can use these suits to test 
oltp capabilities of MatrixOne directly.

# How to use?

This tool is designed to test MatrixOne benchmark for OLTP or any other database functionalities with SQL. 

### 0. Install `sysbench` tool if you don't have it yet.
You can install `sysbench` using the following command:

[Centos]

`yum -y install  make automake libtool pkg-config libaio-dev vim-common`

`yum -y install gcc automake autoconf libtool make`

`yum list`

`yum install sysbench`

[Ubuntu]

`apt-get -y install make automake libtool pkg-config libaio-dev vim-common`

`apt-get install sysbench`


### 1: Run your MatrixOne instance or other DB instance. 

Checkout [Install MatrixOne](https://docs.matrixorigin.io/0.4.0/MatrixOne/Get-Started/install-standalone-matrixone/) to launch a MatrixOne instance.

Or you can launch whatever database software as you want. 

### 2. Fork and clone this mo-tpch project. 

### 3. Run the test.


**Fisrt, prepare test data:**

`sysbench  --mysql-host=127.0.0.1 --mysql-port=6001 --mysql-user=dump --mysql-password=111 oltp_common.lua  --tables=10 --table_size=100000 --threads=1 --time=30 --report-interval=10 --create_secondary=off  --auto_inc=off prepare`

After this opertion,the `10` tables will be created, and each table will be loaded for `100000` records.



**Second, run oltp test:**

[oltp_read_only]

`sysbench  --mysql-host=127.0.0.1 --mysql-port=6001 --mysql-user=dump --mysql-password=111 --db-ps-mode=disable  oltp_read_only.lua  --tables=10 --table_size=10000 --threads=10 --time=30 --report-interval=10 --create_secondary=off  --auto_inc=off --skip_trx=on --range_selects=off --point_selects=1 run`

[oltp_point_select]

`sysbench  --mysql-host=127.0.0.1 --mysql-port=6001 --mysql-user=dump --mysql-password=111 --db-ps-mode=disable  oltp_point_select.lua  --tables=10 --table_size=10000 --threads=10 --time=30 --report-interval=10 --create_secondary=off  --auto_inc=off --skip_trx=on --range_selects=off --point_selects=1 run`

[oltp_insert]

`sysbench  --mysql-host=127.0.0.1 --mysql-port=6001 --mysql-user=dump --mysql-password=111 --db-ps-mode=disable  oltp_insert.lua  --tables=10 --table_size=10000 --threads=10 --time=30 --report-interval=10 --create_secondary=off  --auto_inc=off --skip_trx=on --range_selects=off --point_selects=1 run`

[oltp_update_index]

`sysbench  --mysql-host=127.0.0.1 --mysql-port=6001 --mysql-user=dump --mysql-password=111 --db-ps-mode=disable  oltp_update_index.lua  --tables=10 --table_size=10000 --threads=10 --time=30 --report-interval=10 --create_secondary=off  --auto_inc=off --skip_trx=on --range_selects=off --point_selects=1 run`

**Last, cleanup test data:**

`sysbench  --mysql-host=127.0.0.1 --mysql-port=6001 --mysql-user=dump --mysql-password=111 oltp_common.lua  --tables=10 --table_size=100000 --threads=1 --time=30 --report-interval=10 --create_secondary=off  --auto_inc=off cleanup`
