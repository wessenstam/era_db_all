.. _deploy_mssql:

----------------
Deploying MS SQL
----------------

Traditional database VM deployment resembles the diagram below. The process generally starts with a IT ticket for a database (from Dev, Test, QA, Analytics, etc.). Next one or more teams will need to deploy the storage resources and VM(s) required. Once infrastructure is ready, a DBA needs to provision and configure database software. Once provisioned, any best practices and data protection/backup policies need to be applied. Finally the database can be handed over to the end user. That's a lot of handoffs, and the potential for a lot of friction.

.. figure:: images/0.png

Whereas with a Nutanix cluster and Era, provisioning and protecting a database should take you no longer than it took to read this intro.

Source MSSQL VM
+++++++++++++++++++++

#. Log in to your  *UserXX*\ **-MSSQLSourceVM** (**Cancel** Shutdown Event Tracker) using the **Launch Console** or RDP:

   - **Username** - Administrator
   - **Password** - Nutanix/4u

   .. note::  This is assigned by the SE leading the Bootcamp

#. Close the Server Manager

#. Disable Windows Firewall for all via the following process:

   - Open a Administrative privileged command line
   - Run the below command

   ::

      netsh advfirewall set allprofiles state off

#. Open SQL Server Management Studio (SSMS), and **Connect** using Windows Authentication.

#. Verify you can browse the **SampleDB**.


Registering Your MSSQL VM
+++++++++++++++++++++++++

Registering a database server with Era allows you to deploy databases to that resource, or to use that resource as the basis for a Software Profile.

You must meet the following requirements before you register a SQL Server database with Era:

- A local user account or a domain user account with administrator privileges on the database server must be provided.
- Windows account or the SQL login account provided must be a member of sysadmin role.
- SQL Server instance must be running.
- Database files must not exist in C:\\ Drive.
- Database must be in an online state.
- Windows remote management (WinRM) must be enabled

.. note::

   Your *XYZ*\ **-MSSQL** VM meets all of these criteria.

#. Open your Era instance via its IP address as found in the Prism UI via **https://<ERA-IP ADDRESS>**

#. In **Era**, select **Database Servers** from the dropdown menu and **List** from the lefthand menu.

   .. figure:: images/11.png

#. Click **+ Register** and fill out the following fields:

   - **Microsoft SQL Server -> Single Node Server VM**
   - **IP Address or Name of VM** - *UserXX*\ **-MSSQLSourceVM**
   - **Windows Administrator Name** - Administrator
   - **Windows Administrator Password** - Nutanix/4u
   - **Instance** - MSSQLSERVER (This should auto-populate after providing credentials)
   - **Connect to SQL Server Admin** - Windows Admin User
   - **User Name** - Administrator

   .. note::

      If **Instance** does not automatically populate, disable the Windows Firewall in your *UserXX*\ **-MSSQLSourceVM** VM.

   .. figure:: images/12a.png

   .. note::

    You can click **API Equivalent** for many operations in Era to enter an interactive wizard providing JSON payload based data you've input or selected within the UI, and examples of the API call in multiple languages (cURL, Python, Golang, Javascript, and Powershell).

    .. figure:: images/17.png

#. Click **Register** to begin ingesting the Database Server into Era.

#. Select **Operations** to monitor the registration. This process should take approximately 5 minutes.

   .. figure:: images/13.png

   .. note::

      It is also possible to register existing databases on any server, which will also register the database server it is on.

Creating A Software Profile
+++++++++++++++++++++++++++

Before additional SQL Server VMs can be provisioned, a Software Profile must first be created from the database server VM registered in the previous step. A software profile is a template that includes the SQL Server database and operating system. This template exists as a hidden, cloned disk image on your Nutanix storage.

#. Select **Profiles** from the dropdown menu and **Software** from the lefthand menu.

   .. figure:: images/14.png

#. Click **+ Create** and fill out the following fields:

   - **Microsoft SQL Server**
   - **Name** - *Initials*\ _MSSQL_2016
   - **Description** - (Optional)
   - **Software Profile Version Name** - AUTOPOPULATED
   - **Software Profile Version Description** - (Optional)
   - **Database Server** - Select your registered *Initials*\ -MSSQL VM

   .. figure:: images/15a.png

#. Click **Next** and fill out optionally the Notes, then click **Create**.

#. Select **Operations** to monitor the registration. This process should take approximately 5 minutes.

   .. figure:: images/16a.png

   .. note::

       If creating a profile from a not-cleanly shutdown server it may be corrupt or may not provision successfully. Please ensure that the DBServer had a clean shutdown and clean startup before registering profile to Era.
