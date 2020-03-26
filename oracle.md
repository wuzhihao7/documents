# 表空间

## 新建表空间

```
CREATE TABLESPACE account_check_space
DATAFILE '/u01/app/oracle/oradata/XE/account_check_space.dbf' size 500m
AUTOEXTEND ON 
NEXT 200M MAXSIZE 20480M 
EXTENT MANAGEMENT LOCAL;
```

## 创建临时表空间

```
CREATE TEMPORARY TABLESPACE account_check_space_temp
TEMPFILE '/u01/app/oracle/oradata/XE/account_check_space_temp.dbf'
SIZE 200M
AUTOEXTEND ON
NEXT 50M MAXSIZE 20480M
EXTENT MANAGEMENT LOCAL;
```

# 用户

## 新建用户

```
CREATE USER account_check IDENTIFIED BY "Check!123" 
DEFAULT TABLESPACE account_check_space 
TEMPORARY TABLESPACE account_check_space_temp;
```

# 授权

```
grant connect, resource,dba to account_check;
```

