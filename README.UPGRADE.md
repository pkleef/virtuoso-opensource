# How to upgrade to newer versions of Virtuoso Open Source

Copyright (C) 1998-2019 OpenLink Software <vos.admin@openlinksw.com>

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Introduction](#introduction)
- [Upgrading from VOS 7.2.x to VOS 7.2.y](#upgrading-from-vos-72x-to-vos-72y)
- [Upgrading from VOS 7.0.x-7.2.0 to VOS 7.2.y](#upgrading-from-vos-70x-720-to-vos-72y)
- [Upgrading from VOS 6.x to VOS 7.2.x](#upgrading-from-vos-6x-to-vos-72x)
- [Upgrading from VOS 5.x to VOS 7.x](#upgrading-from-vos-5x-to-vos-7x)
- [Upgrading between deprecated versions](#upgrading-between-deprecated-versions)
  - [Upgrading from VOS 5.0.x to VOS 5.0.Y](#upgrading-from-vos-50x-to-vos-50y)
  - [Upgrading from VOS 5.0.X to VOS 6.1.0](#upgrading-from-vos-50x-to-vos-610)
  - [Upgrading from VOS 6.0.0(-TP1) to VOS 6.1.0](#upgrading-from-vos-600-tp1-to-vos-610)
  - [Upgrading from VOS 6.1.X to VOS 6.1.Y](#upgrading-from-vos-61x-to-vos-61y)
  - [Upgrading from VOS 6.1.X to VOS 6.1.4](#upgrading-from-vos-61x-to-vos-614)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Introduction

This document contains instructions on how to upgrade your existing Virtuoso database from one release to the next.

The following table shows the timeline of each 6.x and 7.x release.

```
  Release date            Version

  August 15, 2018         v7.2.5
  April 25, 2016          v7.2.4
  December 09, 2015       v7.2.2
  June 24, 2015           v7.2.1
  February 17, 2015       v7.2.0
  February 17, 2014       v7.1.0
  August 02, 2013         v7.0.0

  December 10, 2013       v6.1.8
  July 22, 2013           v6.1.7
  July 30, 2012           v6.1.6
  March 15, 2012          v6.1.5
  March 31, 2011          v6.1.4
  March 30, 2011          v6.1.3
  July 09, 2010           v6.1.2
  March 30, 2010          v6.1.1
  February 03, 2010       v6.1.0
  October 16, 2009        v6.0.0
```

Before upgrading any database it is always a wise precaution to make sure there is a complete backup of the existing
installation.

Always make sure the database has been properly shutdown and the transaction log (virtuoso.trx) is empty (0 bytes) before
performing any of the following updates or upgrades.

If your existing database has not been properly checkpointed prior to starting it with a newer virtuoso engine binary
the following error will be written to the virtuoso.log and virtuoso will refuse to start:

```
  The transaction log file has been produced by server version
  '07.20.XXXX'. The version of this server is '07.20.YYYY'. If the
  transaction log is empty or you do not want to replay it then delete
  it and start the server again. Otherwise replay the log using the
  server of version '07.20.XXXX' and make checkpoint and shutdown
  to ensure that the log is empty, then delete it and start using
  new version.
```

If you have any questions on upgrading an existing Virtuoso installation or any other topic related to Virtuoso Open
Source, we invite you to file a [GitHub Issue Report](https://github.com/openlink/virtuoso-opensource/issues) so our
support staff can assist you.


## Upgrading from VOS 7.2.x to VOS 7.2.y

Upgrading your database to the latest version of 7.2.y is performed automatically when you start the newer engine binary
on your existing database.

The only requirement is that the existing database has been properly checkpointed and shutdown using your previous virtuoso
engine binary and that the existing transaction file (virtuoso.trx) is either empty (0 bytes) or removed.


## Upgrading from VOS 7.0.0 upto 7.2.0 to VOS 7.2.y

Upgrading your database from any version 7.0.x upto 7.2.0 to the latest version of 7.2.y is performed automatically when
you start the newer engine with **one** major exception.

As of engine 07.20.3213 Virtuoso supports `DATETIME` and `xsd:dateTime` values both with and without timezone information,
i.e., both "timezoned" and "timezoneless". This solved several subtle issues when when loading or querying RDF datasets
that have both "timezoned" and "timezoneless" data.

Databases that were created by older engines (pre Virtuoso 7.2.1) will be using a backward compatible mode 0, which means
that all datetime fields are "timezoned" as was the implicit case with the older engine.

Please read the Wiki article on [Virtuoso Timezoneless Datetimes](http://vos.openlinksw.com/owiki/wiki/VOS/VirtTimezoneLessDateTime)
which explains the different modes that Virtuoso 7.2.x supports and also shows the new functions that were added as well
as instructions on how fix subtle bugs in your existing code base.

As the timezoneless modes can only be set when creating a new database, switching modes will require a full dump and
reload of all your data.

Contact us using by filing a [GitHub Issue Report](https://github.com/openlink/virtuoso-opensource/issues) if you have
any questions.


## Upgrading from VOS 6.x to VOS 7.2.x

Databases that were created prior to 6.1.4 will not work with the new 7.2.x engine without major conversion as shown below.

Although from version 6.1.4 onward, the database format should for the most part readable by a 7.2.x binary, we **strongly
recommend** dumping the data from any 6.x based database and re-loading it into a fresh 7.2.x database instance.

A new 7.2.x database instance will use the new column store format which is a fast improvement in query speed, the size
of the RDF_QUAD table as well as more efficient use of system memory.

Regular 6.x RDBMS tables can be dumped with the dbdump tool into scripts that can be replayed using the isql tool into
the 7.x database.

For the 6.x RDF_QUAD table, we have a set of dump/load stored procedures to dump graphs into a set of backup files,
which can then be reloaded into the VOS 7.x database. Contact us at <vos.admin@openlinksw.com> for more info.



## Upgrading from VOS 5.x to VOS 7.x

The database format has substantially changed between these two versions of Virtuoso. To upgrade your database, you **must**
dump all your data from your VOS 5.x database and re-load it into VOS 7.x.

Regular VOS 5.x RDBMS tables can be dumped with the dbdump tool into scripts that can be replayed using the isql tool
into the VOS 7.x database.

For VOS 5.0 RDF_QUAD table, we have a set of dump/load stored procedures to dump graphs into a set of backup files,
which can then be reloaded into the VOS 7.x database. Contact us at <vos.admin@openlinksw.com> for more info.

If you attempt to start a VOS 5.x database with a VOS 7.x server, the server will print the following message ans refuses
to start the database:

```
  The database you are opening was last closed with a server of
  version 3016. The present server is of version 3126. This server
  does not read this pre 6.0 format.
```


## Upgrading between deprecated versions

### Upgrading from VOS 5.0.x to VOS 5.0.Y

The database format has not changed between various versions of Virtuoso 5.0.X, so from a database standpoint no particular
steps need to be performed before upgrading to the latest version of Virtuoso 5.

The only requirement is that you have properly shutdown the database prior to installing the latest binaries, as the
transaction logs can have a different version tag. In this case the virtuoso server will print the following message and
refuses to start the database:

```
  The transaction log file has been produced by server version
  '05.00.XXXX'. The version of this server is '05.00.YYYY'. If the
  transaction log is empty or you do not want to replay it then delete
  it and start the server again. Otherwise replay the log using the
  server of version '05.00.XXXX' and make checkpoint and shutdown
  to ensure that the log is empty, then delete it and start using
  new version.
```


### Upgrading from VOS 5.0.X to VOS 6.1.0

The database format has substantially changed between these two versions of Virtuoso. To upgrade your database, you must
dump all your data from your VOS 5.0 database and re-load it into VOS 6.0.

Regular VOS 5.0 RDBMS tables can be dumped with the dbdump tool into scripts that can be replayed using the isql tool
into the VOS 6.0 database.

For VOS 5.0 RDF_QUAD table, we have a set of dump/load stored procedures to dump graphs into a set of backup files,
which can then be reloaded into the VOS 6.0 database. Contact us at <vos.admin@openlinksw.com> for more info.

If you attempt to start a VOS 5.0 database with a VOS 6.0 server, the server will print the following message ans refuses
to start the database:

```
  The database you are opening was last closed with a server of
  version 3016. The present server is of version 3126. This server
  does not read this pre 6.0 format.
```



### Upgrading from VOS 6.0.0(-TP1) to VOS 6.1.0

The database disk format has not changed, but the introduction of a newer RDF index requires you run a script to upgrade
the RDF_QUAD table. Since this can be a lengthy task and take extra disk space (upto 2x space of original RDF_QUAD table
during conversion) this is not done automatically on startup.

After upgrading the binary you cannot perform any SPARQL queries until the new RDF_QUAD table is installed. The steps
for this are:

  1. Shutdown the database and verify .trx file is empty

  2. Check to make sure you have enough disk space to perform operation

  3. Check to make sure you have a proper backup of the database

  4. Edit virtuoso.ini and disable VADInstallDir and possibly the
     HTTPServer section for duration of the upgrade.

  5. Start up the database

  6. Connect with isql to your database and run the libsrc/Wi/clrdf23.sql
     script. Depending on the number of quad records this can take several hours. Once the
     conversion is complete, the database will shutdown itself.

  7. Edit virtuoso.ini and enable VADInstallDir and possibly HTTPServer section

  8. Start up database


### Upgrading from VOS 6.1.X to VOS 6.1.Y

The database format has not changed between various versions of Virtuoso 6.1.X, so from a database standpoint no particular
steps need to be performed before upgrading to the latest version of Virtuoso 6.1.x.

The only requirement is that you have properly shutdown the database prior to installing the latest binaries, as the
transaction logs can have a different version tag. In this case the virtuoso server will print the following message and
refuses to start the database:

```
  The transaction log file has been produced by server version
  '06.01.XXXX'. The version of this server is '06.01.YYYY'. If the
  transaction log is empty or you do not want to replay it then delete
  it and start the server again. Otherwise replay the log using the
  server of version '06.01.XXXX' and make checkpoint and shutdown
  to ensure that the log is empty, then delete it and start using
  new version.
```


### Upgrading from VOS 6.1.X to VOS 6.1.4

In Virtuoso versions before 6.1.4 some XML data was stored in the QUAD store in such a way it could break the sequence
in an index causing wrong results to be passed back.

To fix this we've added an automated check into 6.1.4 to detect if this condition occurs and to fix the index when needed.

When a DBA starts the database with this newer virtuoso binary the following text will appear in the virtuoso session log:

```
  21:05:36 PL LOG: This database may contain RDF data that could cause indexing problems on previous versions of the server.
  21:05:36 PL LOG: The content of the DB.DBA.RDF_QUAD table will be checked and an update may automatically be performed if
  21:05:36 PL LOG: such data is found.
  21:05:36 PL LOG: This check will take some time but is made only once.
```

This check will take some time depending on the number of stored QUADS, but if the check succeeds the following message is
entered in the log and virtuoso will flag this check is done, so it will not affect subsequent restarts of the database server.

```
    21:05:36 PL LOG: No need to update DB.DBA.RDF_QUAD
```

The database will continue to perform the startup routines and go into an online state.

However if the condition is detected, the following message will appear in the log and the virtuoso server will refuse
to start up:

```
  21:05:36 PL LOG: An update is required.
  21:05:36 PL LOG:
  21:05:36 PL LOG: NOTICE: Before Virtuoso can continue fixing the DB.DBA.RDF_QUAD table and its indexes
  21:05:36 PL LOG:         the DB Administrator should check make sure that:
  21:05:36 PL LOG:
  21:05:36 PL LOG:          * there is a recent backup of the database
  21:05:36 PL LOG:          * there is enough free disk space available to complete this conversion
  21:05:36 PL LOG:          * the database can be offline for the duration of this conversion
  21:05:36 PL LOG:
  21:05:36 PL LOG:         Since the update can take a considerable amount of time on large databases
  21:05:36 PL LOG:         it is advisable to schedule this at an appropriate time.
  21:05:36 PL LOG:
  21:05:36 PL LOG: To continue the DBA must change the virtuoso.ini file and add the following flag:
  21:05:36 PL LOG:
  21:05:36 PL LOG:     [Parameters]
  21:05:36 PL LOG:     AnalyzeFixQuadStore = 1
  21:05:36 PL LOG:
  21:05:36 PL LOG: For additional information please contact OpenLink Support <support@openlinksw.com>
  21:05:36 PL LOG: This process will now exit.
```

Since the update will take a substantial amount of time and additional disk space depending on the size of the QUAD store,
OpenLink has decided not to automatically start the update process but hand control back to the DBA and let him decide
when to perform this update. In case the DBA wants to delay the update until a more appropriate time, he should restart
with the previous binary as this latest binary will not allow him to startup.

Once the DBA has checked the backups, disk space and found an appropriate time-slot to run this update, he should edit
the virtuoso.ini file and add the following flag:

```
  [Parameters]
  AnalyzeFixQuadStore = 1
```


Upon starting the virtuoso server, the following messages will appear in the virtuoso.log file:

```
  21:05:57 PL LOG: This database may contain RDF data that could cause indexing problems on previous versions of the server.
  21:05:57 PL LOG: The content of the DB.DBA.RDF_QUAD table will be checked and an update may automatically be performed if
  21:05:57 PL LOG: such data is found.
  21:05:57 PL LOG: This check will take some time but is made only once.
  21:05:57 PL LOG:
  21:05:57 PL LOG: An update is required.
  21:05:57 PL LOG: Please be patient.
  21:05:57 PL LOG: The table DB.DBA.RDF_QUAD and two of its additional indexes will now be patched.
  21:05:57 PL LOG: In case of an error during the operation, delete the transaction log before restarting the server.
  21:05:57 Checkpoint started
  21:05:57 Checkpoint finished, log off
  21:05:57 PL LOG: Phase 1 of 9: Gathering statistics ...
  21:05:58 PL LOG:  * Index sizes before the processing: 002565531 RDF_QUAD, 002565531 POGS, 001171100 OP
  21:05:58 PL LOG: Phase 2 of 9: Copying all quads to a temporary table ...
  21:07:26 PL LOG: * Index sizes of temporary table: 001171100 OP
  21:07:26 PL LOG: Phase 3 of 9: Cleaning the quad storage ...
  21:07:51 PL LOG: Phase 4 of 9: Refilling the quad storage from the temporary table...
  21:09:17 PL LOG: Phase 5 of 9: Cleaning the temporary table ...
  21:09:41 PL LOG: Phase 6 of 9: Gathering statistics again ...
  21:09:41 PL LOG: * Index sizes after the processing: 002565531 RDF_QUAD, 002565531 POGS, 001171100 OP
  21:09:41 PL LOG: Phase 7 of 9: integrity check (completeness of index RDF_QUAD_POGS of DB.DBA.RDF_QUAD) ...
  21:10:00 PL LOG: Phase 8 of 9: integrity check (completeness of primary key of DB.DBA.RDF_QUAD) ...
  21:10:17 PL LOG: Phase 9 of 9: final checkpoint...
  21:10:20 Checkpoint started
  21:10:22 Checkpoint finished, log off
  21:10:22 PL LOG: Update complete.
```

If the update process detects any problem, it will put some debug output into the virtuoso.log and exit. At this point
the DBA is advised to remove the virtuoso.trx file and contact OpenLink Support <support@openlinksw.com>.

After the update process has completed, the database is left in an online state.
