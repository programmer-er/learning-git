# SQL DDL

## CREATE TABLE

<br>

### without constraints

---

```sql
create table TRANSPORT (
    ROUTE int,
    DESTINATION varchar(20),
    DRIVER varchar(20)
);
```

**mysql> desc TRANSPORT;**

| Field       | Type        | Null | Key | Default | Extra |
| ----------- | ----------- | ---- | --- | ------- | ----- |
| ROUTE       | int(11)     | YES  |     | NULL    |       |
| DESTINATION | varchar(20) | YES  |     | NULL    |       |
| DRIVER      | varchar(20) | YES  |     | NULL    |       |

<br><br>

### with some constraints

---

```sql
create table ADMIN (
    ADNO int PRIMARY KEY,
    NAME varchar(20) DEFAULT 'pending',
    CLASS char(3),
    BUS int,
    foreign key (BUS) references TRANSPORT(ROUTE)
);
```

**mysql> desc ADMIN;**

| Field | Type        | Null | Key | Default | Extra |
| ----- | ----------- | ---- | --- | ------- | ----- |
| ADNO  | int(11)     | NO   | PRI | NULL    |       |
| NAME  | varchar(20) | YES  |     | pending |       |
| CLASS | char(3)     | YES  |     | NULL    |       |
| BUS   | int(11)     | YES  | MUL | NULL    |       |

_Note: this can only be executed if TRANSPORT.ROUTE is the PRIMARY KEY of that table._

<br><br>

## ALTER TABLE DESIGN

<br>

### rename column

---

```sql
alter table ADMIN
change CLASS CLASSSEC varchar(10);
```

**mysql> desc admin;**

| Field    | Type        | Null | Key | Default | Extra |
| -------- | ----------- | ---- | --- | ------- | ----- |
| ADNO     | int(11)     | NO   | PRI | NULL    |       |
| NAME     | varchar(20) | YES  |     | pending |       |
| CLASSSEC | varchar(10) | YES  |     | NULL    |       |
| BUS      | int(11)     | YES  | MUL | NULL    |       |

_Note: the datatype can also be changed along with name in the same command if required._
<br><br>

### change datatype

---

```sql
alter table ADMIN
modify ADNO char(5);
```

**mysql> desc ADMIN;**

| Field    | Type        | Null | Key | Default | Extra |
| -------- | ----------- | ---- | --- | ------- | ----- |
| ADNO     | char(5)     | NO   | PRI |         |       |
| NAME     | varchar(20) | YES  |     | pending |       |
| CLASSSEC | varchar(10) | YES  |     | NULL    |       |
| BUS      | int(11)     | YES  | MUL | NULL    |       |

<br><br>

### reorder columns

---

```sql
alter table TRANSPORT
modify DESTINATION varchar(20) after DRIVER;
```

**mysql> desc TRANSPORT;**

| Field       | Type        | Null | Key | Default | Extra |
| ----------- | ----------- | ---- | --- | ------- | ----- |
| ROUTE       | int(11)     | NO   | PRI | NULL    |       |
| DRIVER      | varchar(20) | YES  |     | NULL    |       |
| DESTINATION | varchar(20) | YES  |     | NULL    |       |

<br>

```sql
alter table TRANSPORT
modify DRIVER varchar(15) first;
```

**mysql> desc TRANSPORT;**

| Field       | Type        | Null | Key | Default | Extra |
| ----------- | ----------- | ---- | --- | ------- | ----- |
| DRIVER      | varchar(15) | YES  |     | NULL    |       |
| ROUTE       | int(11)     | NO   | PRI | NULL    |       |
| DESTINATION | varchar(20) | YES  |     | NULL    |       |

_Note: either the AFTER option or FIRST option can be used depending on requirement._

<br><br>

### add table constraints (primary key, unique, foreign key)

---

```sql
alter table TRANSPORT
add unique (DRIVER);
```

**mysql> DESC TRANSPORT;**

| Field       | Type        | Null | Key | Default | Extra |
| ----------- | ----------- | ---- | --- | ------- | ----- |
| DRIVER      | varchar(15) | YES  | UNI | NULL    |       |
| ROUTE       | int(11)     | NO   | PRI | NULL    |       |
| DESTINATION | varchar(20) | YES  |     | NULL    |       |

<br>

```sql
alter table ADMIN
add foreign key (BUS) references TRANSPORT(ROUTE);
```

_Note: you only need to use this if ADMIN.BUS is not already a foreign key._

<br><br>

### add other constraints (not null, check, unique, default)

---

```sql
alter table TRANSPORT
modify DESTINATION varchar(20) not null;
```

**mysql> desc TRANSPORT;**

| Field       | Type        | Null | Key | Default | Extra |
| ----------- | ----------- | ---- | --- | ------- | ----- |
| DRIVER      | varchar(15) | YES  | UNI | NULL    |       |
| ROUTE       | int(11)     | NO   | PRI | NULL    |       |
| DESTINATION | varchar(20) | NO   |     | NULL    |       |

<br>

```sql
alter table TRANSPORT
modify DRIVER varchar(15) default 'TBD';
```

**mysql> desc TRANSPORT;**

| Field       | Type        | Null | Key | Default | Extra |
| ----------- | ----------- | ---- | --- | ------- | ----- |
| DRIVER      | varchar(15) | YES  | UNI | TBD     |       |
| ROUTE       | int(11)     | NO   | PRI | NULL    |       |
| DESTINATION | varchar(20) | NO   |     | NULL    |       |

<br><br>

### remove primary key constraint

---

```sql
alter table TRANSPORT
drop primary key;
```

**mysql> desc TRANSPORT;**

| Field       | Type        | Null | Key | Default | Extra |
| ----------- | ----------- | ---- | --- | ------- | ----- |
| DRIVER      | varchar(15) | YES  | UNI | TBD     |       |
| ROUTE       | int(11)     | NO   |     | NULL    |       |
| DESTINATION | varchar(20) | NO   |     | NULL    |       |

<br><br>

### remove other constaints

---

```sql
alter table TRANSPORT
modify DRIVER varchar(15);
```

**mysql> desc TRANSPORT;**

| Field       | Type        | Null | Key | Default | Extra |
| ----------- | ----------- | ---- | --- | ------- | ----- |
| DRIVER      | varchar(15) | YES  | UNI | NULL    |       |
| ROUTE       | int(11)     | NO   |     | NULL    |       |
| DESTINATION | varchar(20) | NO   |     | NULL    |       |

_Note: to remove other constraints, just modify the column and omit the constraints from the new definition._

<br><br>

### deleting a column

---

```sql
alter table ADMIN
drop CLASSSEC;
```

**mysql> desc ADMIN;**

| Field | Type        | Null | Key | Default | Extra |
| ----- | ----------- | ---- | --- | ------- | ----- |
| ADNO  | char(5)     | NO   | PRI |         |       |
| NAME  | varchar(20) | YES  |     | pending |       |
| BUS   | int(11)     | YES  | MUL | NULL    |       |

<br><br>

### deleting a table

---

```sql
drop table TRANSPORT;
```

**mysql> desc TRANSPORT;**<br>
`ERROR 1146 (42S02): Table 'test.transport' doesn't exist`
