# Hibernate

Copyright (C) 1998-2018 OpenLink Software <vos.admin@openlinksw.com>

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [How to build the Virtuoso Hibernate dialect](#how-to-build-the-virtuoso-hibernate-dialect)
- [Virtuoso dialect sample](#virtuoso-dialect-sample)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## How to build the Virtuoso Hibernate dialect

  * Create a lib directory 

  * Put the following .jar file from the project at
    http://www.hibernate.org in here:

```
	hibernate3.jar
```

  * Run the make command


## Virtuoso dialect sample

```
    hibernate.dialect=virtuoso.hibernate.VirtuosoDialect
    hibernate.connection.driver_class=virtuoso.jdbc3.Driver
    hibernate.connection.url=jdbc:virtuoso://localhost:1111/UID=dba/PWD=dba
```
