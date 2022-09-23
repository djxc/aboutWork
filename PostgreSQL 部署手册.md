## PostgreSQL 部署手册

### PostgreSQL 部署

#### 安装命令

```shell
# Install the repository RPM:
sudo yum install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm

# Install PostgreSQL:
sudo yum install -y postgresql12-server

# Optionally initialize the database and enable automatic start:
sudo /usr/pgsql-12/bin/postgresql-12-setup initdb
sudo systemctl enable postgresql-12
sudo systemctl start postgresql-12
```

#### 用户管理

```shell
# Swich user for postgres
su - postgres

# Login db with postgres 
psql -U postgres

# Change password
ALTER USER postgres WITH PASSWORD 'Pwd_2018';

# Quit db
\q
```

#### 远程访问

```shell
# Add port:5432 in firewall
sudo firewall-cmd --add-port=5432/tcp --permanent
sudo firewall-cmd --reload

# Config listen_address="*"
vim /var/lib/pgsql/12/data/postgresql.conf
# what IP address(es) to listen on;
# comma-separated list of addresses;
# defaults to 'localhost'; use '*' for all
listen_addresses = '*'

# config data_directory=''
data_directory = ''

# Config host
vim /var/lib/pgsql/12/data/pg_hba.conf
# Add IPv4 local connections:
host    all             all             0.0.0.0/0               trust

# Restart postgresql-12
sudo systemctl restart postgresql-12
```



### PostgreSQL Extension 安装

#### pgRouting

pgRouting extends the PostGIS / PostgreSQL geospatial database

```shell
# Swich user for postgres
su - ${role}

# Install pgRouting extension for geospatial routing
sudo yum install pgrouting_12 -y

# Restart postgresql-12
sudo systemctl restart postgresql-12
```



#### postgis

```shell
# Install postgis depencies
yum -y install epel-release

# Install postgis12
yum -y install postgis30_12 postgis30_12-utils postgis30_12-client
```

**create extension**

```sql
-- Enable PostGIS (as of 3.0 contains just geometry/geography)
CREATE EXTENSION postgis;
-- enable raster support (for 3+)
CREATE EXTENSION postgis_raster;
-- Enable Topology
CREATE EXTENSION postgis_topology;
-- Enable PostGIS Advanced 3D
-- and other geoprocessing algorithms
-- sfcgal not available with all distributions
CREATE EXTENSION postgis_sfcgal;
-- fuzzy matching needed for Tiger
CREATE EXTENSION fuzzystrmatch;
-- rule based standardizer
CREATE EXTENSION address_standardizer;
-- example rule data set
CREATE EXTENSION address_standardizer_data_us;
-- Enable US Tiger Geocoder
CREATE EXTENSION postgis_tiger_geocoder;
```





### 基础运维命令

#### 创建数据库

```sql
-- Create DB
createdb ${db name}

-- Drop DB
dropdb ${db name}

-- Connect DB
psql -d ${db name}
```









### 注意事项

#### 用户名

PostgreSQL 用户名是和操作系统用户账号分开的。 如果你连接到一个数据库时，你可以选择以何种PostgreSQL 用户名进行联接。

如果你不选择，那么缺省就是你的当前操作系统账号。 如果这样，那么总有一个与操作系统用户同名的 PostgreSQL 用户账号用于启动服务器， 并且通常这个用户都有创建数据库的权限。如果你不想以该用户身份登录， 那么你也可以在任何地方声明一个 -U 选项以选择一个用于连接的 PostgreSQL 用户名。