.. _admin_mssqldb:

--------------------------
DB Administration with Era
--------------------------

We will now see how to perform normal database admin task with Era.

**In this lab you will Administor your MSSQL Fiesta DB**

.. Register Your Database
   ++++++++++++++++++++++

   #. In **Era**, select **Databases** from the dropdown menu and **Sources** from the lefthand menu.

      .. figure:: images/1a.png

   #. Click **+ Register** and fill out the following fields:

      - **Microsoft SQL Server -> Database**
      - **Database is on a Server that is:** - Registered
      - **Registered Database Servers** - Select your registered *Initials*\ -MSSQL2 VM

      .. figure:: images/2.png

      - **Unregistered Databases** - Select SampleDB
      - **Database Name in Era** - *Initials*\ -LABSQLDB

      .. figure:: images/3.png

      - **Recovery Model** - Full
      - **Manage Log Backups with** - Era
      - **Name** - *Initials*\ -MSSQL_TM
      - **SLA** - DEFAULT_OOB_BRASS_SLA (no continuous replay)

      .. figure:: images/4.png

   #. Click **Register**

   #. Select **Operations** from the dropdown menu to monitor the registration. This process should take approximately 5 minutes.

Snapshot Your Database
++++++++++++++++++++++

Before we take a manual snapshot of our Database, lets create a new table into FiestaDB

Write New Table Into Database
.............................

#. RDP/Console into your *Initials*\ -MSSQL2 VM

#. Open SQL Server Managment Studio (SSMS), and **Connect** using Windows Authentication.

#. Right-Click on **xyz-fiesta** and Select **New Query**.

   .. figure:: images/5a.png

#. **Execute** the following SQL Query:

   .. code-block:: sql

      select * into dbo.testlabtable from stores;

   .. figure:: images/6a.png

#. Verify the new table is there by doing a refresh on **Tables**. Use **right click on Tables, under xyz-fiesta, -> Refresh**.

   .. figure:: images/6b.png

Take Manual Snapshot of Database
................................

#. In **Era**, select **Databases** from the dropdown menu and **Sources** from the lefthand menu.

#. Click on the Time Machine for your Database *Initials*\ -fiesta_TM

   .. figure:: images/7a.png

#. Once that is complete, click **Actions > Snapshot**.

   .. Figure:: images/8a.png

   - **Snapshot Name** - *Initials*\ -fiesta-1st-Snapshot

   .. Figure:: images/9a.png

#. Click **Create**

#. Select **Operations** to monitor the registration. This process should take approximately 2-5 minutes.

Clone Your Database Server & Database
+++++++++++++++++++++++++++++++++++++

A lot of organisations want to test upgrades, applications and other materials against a Production databases. One way to solve this issue, as no organisation would allow that, is to create a clone from the production database and run all tests against that cloned Database. This part of the workshop is going to simpulate such a scenario and explains how Era helps to set this up in an easy manner.

#. In **Era**, select **Time Machines** from the dropdown menu and select *Initials*\ -fiesta_TM

#. Click **Actions > Create Database Clone > Database**.

   - **Nutanix Cluster** - EraCluster
   - **Snapshot** - *Initials*\ -fiesta-1st-Snapshot (Date Time)
   
   .. figure:: images/10a.png

#. Click **Next**

   - **Database Server** - Create New Server
   - **Database Server Name** - *Initials*\ -MSSQL2_Dev
   - **Description** - (Oprional)
   - **Compute Profile** - DEFAULT_OOB_COMPUTE
   - **Network Profile** - Primary-MSSQL-Network
   - **Administrator Password** - Nutanix/4u

   .. figure:: images/11a.png

#. Click **Next**

   - **Clone Name** - *Initials*\ -fiesta_dev
   - **Description** - (Optional)
   - **Database Name on VM** - *Initials*-fiesta_dev
   - **Instance Name** - MSSQLSERVER

   .. figure:: images/12a.png

#. Click **Clone**

#. Select **Operations** to monitor the registration. This process should take approximately 15-20 minutes.

#. Wait till the Clone operation hs been completed before moving to the next part of the workshop.

Delete Table and Clone Refresh
++++++++++++++++++++++++++++++

As mentioned earlier, let's run some destructive tests on the Cloned Database Server VM and its database fiesta_dev. After that we want to get the original database back as it was before the descructive test. We will delete a table and use the **Era Clone Refresh** action from the last snapshot we took to get the table back in our testing Database.

Delete Table
............

#. RDP/Console into your *Initials*\ -MSSQL2_Dev VM

#. Open SQL Server Managment Studio (SSMS), and **Connect** using Windows Authentication.

#. Expand **fiesta_dev > Tables**, Right-Click on **dbo.testlabtable** and Select **Delete** and **OK**.

Clone Refresh
.............

#. In **Era**, select **Databases** from the dropdown menu and **Clones** from the lefthand menu.

#. Select the Clone for your Database *Initials*\ -fiesta_dev and Click **Refresh**.

   - **Nutanix Cluster** - EraCluster
   - **Snapshot** - *Initials*\ -fiesta-1st-Snapshot (Date Time)


#. Click **Refresh**

#. Select **Operations** to monitor the registration. This process should take approximately 2-5 minutes.

   .. note::
      To see the steps, from MS SQL's PoV, you can click on the Refresh button (in the SQL Server Managment Studio (SSMS)) during the Refresh operation from Era. You will see:
      
      - No fiesta_dev database
      - Restoring fiesta_dev database
      - Availablility of the fiesta_dev database

Verify Table is Back
....................

#. RDP/Console into your *Initials*\ -MSSQL2_Dev VM

#. Open SQL Server Managment Studio (SSMS), and **Connect** using Windows Authentication.

#. Expand **fiesta_dev > Tables**, verify **dbo.testlabtable** is there.
