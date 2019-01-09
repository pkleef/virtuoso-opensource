<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [How to build Virtuoso JDBC 2.0 SSL Driver](#how-to-build-virtuoso-jdbc-20-ssl-driver)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

How to build Virtuoso JDBC 2.0 SSL Driver
=========================================

The Virtuoso JDBC 2.x SSL Driver can be still be build using JDK 1.3

  * cd libsrc/JDBCDriverType4

  * mkdir security

  * Download the SUN Java Secure Socket Extension (JSSE) 1.0.3
    package from:

```
	  http://java.sun.com/products/archive/jsse 
```

  and copy the following jar files into the security directoy

```
	jcert.jar
	jnet.jar
	jsse.jar
```

  * cd libsrc/JDBCDriverType4
  
  * make jdk2-target-ssl
