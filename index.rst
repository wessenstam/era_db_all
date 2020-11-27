.. title:: Databases: Era with Database NEEUR Bootcamp


.. toctree::
  :maxdepth: 2
  :caption: General labsetup
  :name: _era_lab
  :hidden:

  labsetup/labsetup


.. toctree::
  :maxdepth: 2
  :caption: Era with Postgres
  :name: _era_postgres
  :hidden:

  postgress/era_provision_postgresdb/era_provision_postgresdb
  postgress/era_clone_postgresdb/era_clone_postgresdb

.. toctree::
  :maxdepth: 2
  :caption: Era with Oracle
  :name: _era_oracle
  :hidden:

  oracle/deploy_oracle/deploy_oracle
  oracle/deploy_oracle_era/deploy_oracle_era
  oracle/patching_oracle/patching_oracle
  oracle/admin_oracle/admin_oracle

.. toctree::
  :maxdepth: 2
  :caption: Era with MS SQL
  :name: _era_mssql
  :hidden:

  mssql/deploy_mssql/deploy_mssql
  mssql/deploy_mssql_era/deploy_mssql_era
  mssql/admin_mssqldb/admin_mssqldb


.. toctree::
  :maxdepth: 2
  :caption: Optional Labs
  :name: _optional_labs
  :hidden:

  mssql/hammerdb/hammerdb
  era_rest_api/era_rest_api

.. toctree::
  :maxdepth: 2
  :caption: Appendix
  :name: _appendix_general
  :hidden:

  tools_vms/windows_tools_vm
  tools_vms/linux_tools_vm
  appendix/glossary
  

.. _getting_started:

---------------
Getting Started
---------------

Welcome to the Databases: Era with Databases Bootcamp! This workbook accompanies an instructor-led session that introduces Nutanix technologies and many common management tasks.


What's New
++++++++++

- Workshop updated for the following software versions:
    - AOS 5.11.x / 5.15.x / 5.16.x / 5.17.x / 5.18.x
    - PC 2020.9

- Optional Lab Updates:

Agenda
++++++

- Introductions
- Lab Setup
- Postgress

  - Provision a PostgresSQL Server
  - Clone a PostgresSQL Server

- Oracle

  - Register an Oracle Server
  - Deploy an Oracle Server
  - Patch an Oracle Server
  - Refresh a cloned Oracle Server 

- Microsoft SQL Server

  - Register a MS SQl Server
  - Deploy a MS SQL Server with Era
  - Refresh a cloned MS SQL Database


Optional labs:

- Era API Explorer (Only for Oracle and Postgress)
- MS SQL Server Performance testing using HammerDB in relation to the MS SQL Best Practices Guide

Introductions
+++++++++++++

- Name
- Familiarity with Nutanix

Initial Setup
+++++++++++++

- Take note of the *Passwords* being used.
- Log into your virtual desktops (connection info below)

Environment Details
+++++++++++++++++++

Nutanix Workshops are intended to be run in the Nutanix Hosted POC environment. Your cluster will be provisioned with all necessary images, networks, and VMs required to complete the exercises.

Networking
..........

Hosted POC clusters follow a standard naming convention:

- **Cluster Name** - POC\ *XYZ*
- **Subnet** - 10.**21**.\ *XYZ*\ .0
- **Cluster IP** - 10.**21**.\ *XYZ*\ .37

If provisioned from the marketing pool:

- **Cluster Name** - MKT\ *XYZ*
- **Subnet** - 10.**20**.\ *XYZ*\ .0
- **Cluster IP** - 10.**20**.\ *XYZ*\ .37

For example:

- **Cluster Name** - POC055
- **Subnet** - 10.21.55.0
- **Cluster IP** - 10.21.55.37

Throughout the Workshop there are multiple instances where you will need to substitute *XYZ* with the correct octet for your subnet, for example:

.. list-table::
   :widths: 25 75
   :header-rows: 1

   * - IP Address
     - Description
   * - 10.21.\ *XYZ*\ .37
     - Nutanix Cluster Virtual IP
   * - 10.21.\ *XYZ*\ .39
     - **PC** VM IP, Prism Central
   * - 10.21.\ *XYZ*\ .40
     - **DC** VM IP, NTNXLAB.local Domain Controller

Each cluster is configured with 2 VLANs which can be used for VMs:

.. list-table::
  :widths: 25 25 10 40
  :header-rows: 1

  * - Network Name
    - Address
    - VLAN
    - DHCP Scope
  * - Primary
    - 10.21.\ *XYZ*\ .1/25
    - 0
    - 10.21.\ *XYZ*\ .50-10.21.\ *XYZ*\ .124
  * - Secondary
    - 10.21.\ *XYZ*\ .129/25
    - *XYZ1*
    - 10.21.\ *XYZ*\ .132-10.21.\ *XYZ*\ .253

Credentials
...........

.. note::

  The *<Cluster Password>* is unique to each cluster and will be provided by the leader of the Workshop.

.. list-table::
   :widths: 25 35 40
   :header-rows: 1

   * - Credential
     - Username
     - Password
   * - Prism Element
     - admin
     - *<Cluster Password>*
   * - Prism Central
     - admin
     - *<Cluster Password>*
   * - Controller VM
     - nutanix
     - *<Cluster Password>*
   * - Prism Central VM
     - nutanix
     - *<Cluster Password>*

Each cluster has a dedicated domain controller VM, **DC**, responsible for providing AD services for the **NTNXLAB.local** domain. The domain is populated with the following Users and Groups:

.. list-table::
   :widths: 25 35 40
   :header-rows: 1

   * - Group
     - Username(s)
     - Password
   * - Administrators
     - Administrator
     - nutanix/4u
   * - SSP Admins
     - adminuser01-adminuser25
     - nutanix/4u
   * - SSP Developers
     - devuser01-devuser25
     - nutanix/4u
   * - SSP Consumers
     - consumer01-consumer25
     - nutanix/4u
   * - SSP Operators
     - operator01-operator25
     - nutanix/4u
   * - SSP Custom
     - custom01-custom25
     - nutanix/4u
   * - Bootcamp Users
     - user01-user25
     - nutanix/4u

Access Instructions
+++++++++++++++++++

The Nutanix Hosted POC environment can be accessed a number of different ways:

Lab Access User Credentials
...........................

PHX Based Clusters:
**Username:** PHX-POCxxx-User01 (up to PHX-POCxxx-User20), **Password:** *<Provided by Instructor>*

RTP Based Clusters:
**Username:** RTP-POCxxx-User01 (up to RTP-POCxxx-User20), **Password:** *<Provided by Instructor>*

Frame VDI
.........

Login to: https://frame.nutanix.com/x/labs

**Nutanix Employees** - Use your **NUTANIXDC** credentials
**Non-Employees** - Use **Lab Access User** Credentials

Parallels VDI
.................

PHX Based Clusters Login to: https://xld-uswest1.nutanix.com

RTP Based Clusters Login to: https://xld-useast1.nutanix.com

**Nutanix Employees** - Use your **NUTANIXDC** credentials
**Non-Employees** - Use **Lab Access User** Credentials

Employee Pulse Secure VPN
..........................

Download the client:

PHX Based Clusters Login to: https://xld-uswest1.nutanix.com

RTP Based Clusters Login to: https://xld-useast1.nutanix.com

**Nutanix Employees** - Use your **NUTANIXDC** credentials
**Non-Employees** - Use **Lab Access User** Credentials

Install the client.

In Pulse Secure Client, **Add** a connection:

For PHX:

- **Type** - Policy Secure (UAC) or Connection Server
- **Name** - X-Labs - PHX
- **Server URL** - xlv-uswest1.nutanix.com

For RTP:

- **Type** - Policy Secure (UAC) or Connection Server
- **Name** - X-Labs - RTP
- **Server URL** - xlv-useast1.nutanix.com


Nutanix Version Info
++++++++++++++++++++

- **AHV Version** - AHV 20170830.337
- **AOS Version** - 5.11.x
- **PC Version** - 5.16.1.2
