# **Migrating Microsoft SQL Server Databases to Nutanix Database Service** 

## Legal 

© 2024 Nutanix, Inc. All rights reserved. Nutanix, the Enterprise Cloud Platform, the Nutanix logo and the other Nutanix products, features, and/or programs mentioned herein are registered trademarks or trademarks of Nutanix, Inc. in the United States and other countries. All other brand and product names mentioned herein are for identification purposes only and are the property of their respective holder(s), and Nutanix may not be associated with, or sponsored or endorsed by such holder(s). This document is provided for informational purposes only and is presented "as is" with no warranties of any kind, whether implied, statutory or otherwise. 

Nutanix, Inc. 1740 Technology Drive, Suite 150 San Jose, CA 95110 

Migrating Microsoft SQL Server Databases to Nutanix Database Service 

## **Contents** 

**1. Executive Summary.................................................................................4 2. Database Migration Steps.......................................................................5** Review Requirements......................................................................................................................... 5 Provision Database in Nutanix Database Service..............................................................................6 Back Up the Source Database...........................................................................................................8 Restore the Database.........................................................................................................................9 Verify the Restored Database.............................................................................................................9 Apply Best Practices.........................................................................................................................10 Post-Migration Steps.........................................................................................................................10 Go Live..............................................................................................................................................13 **About Nutanix.............................................................................................14 List of Figures.............................................................................................................................................15** 

Migrating Microsoft SQL Server Databases to Nutanix Database Service 

## 1. Executive Summary 

This document describes the recommended steps for migrating Microsoft SQL Server databases from legacy infrastructure running on bare metal or virtualized environments other than Nutanix to Nutanix Database Service (NDB). NDB, a database-as-a-service solution, simplifies and automates database life cycle management across on-premises and public clouds for Microsoft SQL Server, Oracle Database, PostgreSQL, MySQL, and MongoDB databases. 

The steps discussed in this document use SQL Server native backup and recovery options to migrate databases according to NDB best practices. Deviating from these steps might cause some of the NDB functionality to be unavailable or to not function as optimally designed. 

For more information on other options available for migrating databases to Nutanix, see Database Migration Strategies on Nutanix or discuss options with the Nutanix Professional Services team. 

You can also perform a database restore using the NDB Build Database from an Existing Backup option through the provisioning workflow. However, this option has some limitations, which are described in the Caution section of the documentation. 

This document is part of the Nutanix Solutions Library. We wrote it for database administrators and architects who design, manage, and support SQL Server databases in NDB environments. Readers should already be familiar with SQL Server and NDB. 

_Table: Document Version History_ 

|**Version Number**|**Published**|**Notes**|
|---|---|---|
|1.0|July 2024|Original publication.|



© 2024 Nutanix, Inc. All rights reserved  | **4** 

Migrating Microsoft SQL Server Databases to Nutanix Database Service 

## 2. Database Migration Steps 

The following sections describe the steps required to migrate SQL Server databases to NDB: 

**1.** Review requirements. 

**2.** Provision a database in NDB. 

**3.** Back up the source database. 

**4.** Restore the database in the target server. 

**5.** Verify the restored database. 

**6.** Apply best practices. 

**7.** Complete post-migration updates. 

**8.** Go live. 

## **Review Requirements** 

Before you migrate the SQL Server database, complete the following prerequisites: 

- Review the Microsoft SQL Server on Nutanix best practice guide and Database Migration Strategies on Nutanix tech note. 

- Understand the amount of downtime you can have when migrating the database and align the migration strategy accordingly (for example, full backup only or full backup and differential backups). 

- Gather the current database sizing information and other vital database configuration options, such as collation, maximum degree of parallelism (MAXDROP), and memory configuration. 

- Determine the number of data files the current source database contains. 

- Determine the expected growth of the database. 

- Understand the source database and instance configuration options (for example, collation, memory configuration, database collation). 

© 2024 Nutanix, Inc. All rights reserved  | **5** 

Migrating Microsoft SQL Server Databases to Nutanix Database Service 

## **Provision Database in Nutanix Database Service** 

Based on the requirements, provision a database in NDB using the target database name, the appropriate database, instance size, and relevant configuration options. 

Provisioning the database in NDB provides the following benefits: 

- Reduces the downtime by creating the database VM and the database file system during the maintenance window 

- Provides an option to create the database with the optimal number of disks 

- Provides flexibility during the data file split operation 

In the following example, we provisioned the new database with the name NDB_MigrationDB and 800 GB of space. NDB best practices recommend provisioning a new NDB database using multiple data files. In this scenario, however, the source database is constructed using only two data files, so we must first migrate the source database to the NDB environment and then split the database into multiple data files. For more information, see the Apply Best Practices section. 

Because these post-migration steps accumulate a significant amount of log data, we recommend migrating the source database using the simple recovery mode to reduce the log operations. In our scenario, we configured the target database with the DEFAULT_OOB_BRASS_SLA (simple recovery) time machine. 

## **Operating System Disk Layout** 

After successfully provisioning the target database server in NDB, verify its disk layout in the operating system (OS). For more information, see SQL Server Database Provisioning. 

The new database is provisioned using multiple vDisks. 

Our newly provisioned NDB database contains the following disks: 

- Two disks for tempdb data files 

- One disk for tempdb log file 

- Four disks for target database data files 

- One disk for target database log file 

© 2024 Nutanix, Inc. All rights reserved  | **6** 

Migrating Microsoft SQL Server Databases to Nutanix Database Service 

## **Tempdb Configuration** 

NDB creates all databases using multiple data files equally spread across allocated vDisks, as shown in the following image. The tempdb and the target databases use multiple vDisks and multiple data files according to NDB best practices. The size of the tempdb is subject to the workload requirement expected in your environment. 

For our scenario, we created tempdb using eight data files spread across two data drives and one log file located on the temp log drive. NDB uses mount points to attach multiple virtual disks to the OS. 

Figure 1: Example Mount Point Layout of the Tempdb 

## **Target Database Configuration** 

Because NDB uses multiple virtual disks presented as mount points in the operating system, it builds the target NDB_MigrationDB using 32 data files spread across four virtual disks. A single vDisk contains the database log file. 

© 2024 Nutanix, Inc. All rights reserved  | **7** 

Migrating Microsoft SQL Server Databases to Nutanix Database Service 

Figure 2: Example Mount Point Layout of the Target Database 

## **Back Up the Source Database** 

To back up the source database, complete the following steps: 

## **1.** Verify the source database recovery mode: 

`SELECT NAME,recovery_model_desc FROM sys.databases` 

**2.** If the source database is in full recovery mode, change the recovery to simple recovery mode: 

`ALTER DATABASE hdb4_tpch_100sf SET recovery simple WITH no_wait` 

**3.** Back up the source database using a single full backup and subsequent differential backups according to your requirements. 

In the following example, we used multiple backup files: 

`BACKUP DATABASE [HDB4_TPCH_100SF] TO DISK = N'X:\Backup\HDB4_TPCH_100SF\HDB4_TPCH_100SF1.bak', DISK = N'X:\Backup\HDB4_TPCH_100SF\HDB4_TPCH_100SF2.bak', DISK = N'X:\Backup\HDB4_TPCH_100SF\HDB4_TPCH_100SF3.bak', DISK = N'X:\Backup\HDB4_TPCH_100SF\HDB4_TPCH_100SF4.bak', DISK = N'X:\Backup\HDB4_TPCH_100SF\HDB4_TPCH_100SF5.bak', DISK = N'X:\Backup\HDB4_TPCH_100SF\HDB4_TPCH_100SF6.bak', DISK = N'X:\Backup\HDB4_TPCH_100SF\HDB4_TPCH_100SF7.bak', DISK = N'X:\Backup\HDB4_TPCH_100SF\HDB4_TPCH_100SF8.bak'` 

© 2024 Nutanix, Inc. All rights reserved  | **8** 

Migrating Microsoft SQL Server Databases to Nutanix Database Service 

```
WITH NOFORMAT, NOINIT, NAME = N' HDB4_TPCH_100SF-Full Database Backup',
 SKIP, NOREWIND, NOUNLOAD, COMPRESSION, STATS = 10
GO
```

## **Restore the Database** 

To restore the database, complete the following steps: 

**1.** Move the backup files so that the target database server can access them. 

**2.** Remote desktop to the NDB provisioned target database server and delete the target database NDB_MigrationDB using SQL Server Management Studio (SSMS). 

The target database file structure remains the same. 

**3.** Restore the target database using the source backup files. 

The database data files restore to the remaining file structure created by the NDB-provisioned database. 

**Note:** Because the source database structure is different from the NDB-provisioned database (target), you can't recreate the same data file structure as the target database while restoring the database. However, you can spread the source database data files so that the database uses as many vDisks as possible within the current structure (..\NDB_MIgrationDB\Data1\.., ..\NDB_MIgrationDB\Data2\..). 

Sample database restore process: 

```
USE [master]
RESTORE DATABASE NDB_Migration FROM
  DISK = N'D:\HDB45_TPCH_100SF\HDB4_TPCH_100SF1.bak',
  DISK = N'D:\HDB45_TPCH_100SF\HDB4_TPCH_100SF2.bak',
  DISK = N'D:\HDB45_TPCH_100SF\HDB4_TPCH_100SF3.bak',
  DISK = N'D:\HDB45_TPCH_100SF\HDB4_TPCH_100SF4.bak',
  DISK = N'D:\HDB45_TPCH_100SF\HDB4_TPCH_100SF5.bak',
  DISK = N'D:\HDB45_TPCH_100SF\HDB4_TPCH_100SF6.bak',
  DISK = N'D:\HDB45_TPCH_100SF\HDB4_TPCH_100SF7.bak',
  DISK = N'D:\HDB45_TPCH_100SF\HDB4_TPCH_100SF8.bak',
  WITH FILE = 1,
  MOVE N'HDB4_TPCH_100SF' TO N'C:\NTNX\ERA_DATASES\NDB_MigrationDB\DATA1\data
\NDB_MigrationDB1.ndf',
  MOVE N'HDB4_TPCH_100SF_2' TO N'C:\NTNX\ERA_DATASES\NDB_MigrationDB
\DATA2\data\NDB_MigrationDB2.ndf',
  MOVE N'HDB4_TPCH_100SF_log' TO N'C:\NTNX\ERA_DATASES\NDB_MigrationDB\LOGS
\log\NDB_MigrationDB_log.ldf', NOUNLOAD, STATS = 5
GO
```

## **Verify the Restored Database** 

After the database restores successfully, verify that it's online and accessible in SSMS. Verify that the database layout and configuration restored as expected. 

© 2024 Nutanix, Inc. All rights reserved  | **9** 

Migrating Microsoft SQL Server Databases to Nutanix Database Service 

Figure 3: Example Restored Database Data File Layout 

The newly migrated database and the new file layout, with two data files, replaced the original database provisioned by NDB. 

## **Apply Best Practices** 

The new database layout deviates from NDB best practices because it has fewer data files than recommended by Nutanix. For optimal performance, create additional data files in the database and spread data evenly across all the data files. If you have sufficient downtime, we recommend that you split the database into multiple data files before going live. If you can't split the database all at once, you can achieve the same level of data separation over multiple planned maintenance windows by splitting the database into more than one data file at a time over a few iterations until you achieve the desired outcome. 

For more information on creating additional data files and spreading the data among multiple data files and vDisks in the database, see SQL Server Best Practices: How to Split a Single Datafile. 

Because splitting the database into multiple data files requires downtime and is a timeconsuming operation, consider 18 to 24 months of data growth for the database and structure the database layout accordingly to support the I/O throughput requirements. As a guideline, we suggest including two data files and a single vDisk for each 250 GB of data in the database. For example, you can construct a 1 TB database using eight data files and four vDisks for data space, a single data file, and vDisk for database log space. 

## **Post-Migration Steps** 

After you migrate the database and split the database into multiple data files, verify the migrated database and take a fresh snapshot (full backup) of the migrated database. 

© 2024 Nutanix, Inc. All rights reserved  | **10** 

Migrating Microsoft SQL Server Databases to Nutanix Database Service 

## **Verify the Migrated Database** 

Verify that the migrated database contains the relevant configuration options to support your environment (for example, MAXDROP, database collation, and cardinality settings). 

## **Snapshot the Migrated Database** 

To take a new snapshot, complete the following steps: 

**1.** In the Database Time Machine menu, go to **Actions** > **Snapshot** . 

**2.** Name the snapshot. 

**3.** From the Operations pane, verify that the database's post-migration snapshot completed. 

## **Change Time Machine Service-Level Agreement** 

Next, set the migrated database to full recovery mode. Update the NDB time machine configuration to reflect the changes in the NDB configuration and schedule the log catch up (log backups) for the migrated database. 

In this scenario, we changed the time machine service-level agreement (SLA) so that the database runs in full recovery mode (Gold SLA). 

To change the time machine configuration, complete the following steps: 

**1.** Go to **Actions** > **Update** . 

**2.** Select SLA **DEFAULT_OOB_GOLD_SLA** . 

**3.** Check the Operations log or the time machine configuration details to verify that you updated the time machine. 

© 2024 Nutanix, Inc. All rights reserved  | **11** 

Migrating Microsoft SQL Server Databases to Nutanix Database Service 

Figure 4: New Time Machine Configuration 

© 2024 Nutanix, Inc. All rights reserved  | **12** 

Migrating Microsoft SQL Server Databases to Nutanix Database Service 

Even though you updated the time machine configuration in NDB, the log catch up fails because the restored database isn't set to full recovery mode in the database (SSMS). 

Figure 5: Unsuccessful Operation Log Output 

To set the recovery mode in the database, complete the following steps: 

**1.** Connect to the database using SSMS. 

**2.** Run the following command: 

`USE [master] GO ALTER DATABASE <Database name> SET RECOVERY FULL WITH NO_WAIT GO` 

The log catchup successfully completes. 

You can also verify the log catchup interval schedule in the Time Machine pane. 

## **Go Live** 

After you migrate the database and verify that it's operational in full recovery mode according to your requirements, you can go live with the migrated database. 

© 2024 Nutanix, Inc. All rights reserved  | **13** 

Migrating Microsoft SQL Server Databases to Nutanix Database Service 

## About Nutanix 

Nutanix offers a single platform to run all your apps and data across multiple clouds while simplifying operations and reducing complexity. Trusted by companies worldwide, Nutanix powers hybrid multicloud environments efficiently and cost effectively. This enables companies to focus on successful business outcomes and new innovations. Learn more at Nutanix.com. 

© 2024 Nutanix, Inc. All rights reserved  | **14** 

Migrating Microsoft SQL Server Databases to Nutanix Database Service 

## **List of Figures** 

Figure 1: Example Mount Point Layout of the Tempdb................................................................................. 7 Figure 2: Example Mount Point Layout of the Target Database....................................................................8 Figure 3: Example Restored Database Data File Layout............................................................................ 10 Figure 4: New Time Machine Configuration.................................................................................................12 Figure 5: Unsuccessful Operation Log Output.............................................................................................13 

