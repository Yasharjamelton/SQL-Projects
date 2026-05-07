# SQL-Project (Fulcrum and SQL integration)

Phase 1: Install What You Need
*****Step 1: Install SQL Server******
If you do not already have SQL Server installed, install:
SQL Server Developer Edition
SQL Server Management Studio, also called SSMS
SQL Server is the database engine.
SSMS is the tool you use to create databases, run SQL scripts, and inspect tables.
After installing, open SQL Server Management Studio.
Try connecting to:
localhost
or:
localhost\SQLEXPRESS
depending on how SQL Server was installed.

****Step 2: Create a Database****
In SSMS, click New Query and run:

###CREATE DATABASE FulcrumWarehouse;
###GO
Should keep in mind that :
if you really need the database files inside D:\Yashar projects\SQL-Projects\DATA-SQL SERVER\:
Close SSMS (to release any file locks).
In File Explorer, go to that folder.
Right‑click the folder → Properties → General tab → Advanced….
Uncheck “Compress contents to save disk space” → OK → Apply.
Choose “Apply changes to this folder, subfolders and files”.
Wait for decompression to finish.
Re‑run your CREATE DATABASE command with the full file paths.
This creates a new database called:
FulcrumWarehouse
This database will store your Fulcrum data.

******Step 3: Create Database Schemas*******

A schema is like a folder inside the database.
We will use:
staging
for raw incoming data.
reporting
for cleaned analyst-friendly tables.
Run this in SSMS:

###USE FulcrumWarehouse;
###GO
###CREATE SCHEMA staging;
###GO
###CREATE SCHEMA reporting;
###GO

***Phase 2: Create SQL Tables***
We need two important tables first.

********Step 4: Create the Raw Webhook Events Table*******
This table stores every message received from Fulcrum.

Run this:
##USE FulcrumWarehouse;
##GO

###CREATE TABLE staging.fulcrum_webhook_events (
    event_id INT IDENTITY(1,1) PRIMARY KEY,
    received_at DATETIME2 NOT NULL DEFAULT SYSUTCDATETIME(),
    event_type NVARCHAR(100) NULL,
    fulcrum_record_id NVARCHAR(100) NULL,
    payload NVARCHAR(MAX) NOT NULL,
    processed_at DATETIME2 NULL,
    processing_status NVARCHAR(50) NOT NULL DEFAULT 'pending',
    error_message NVARCHAR(MAX) NULL
);

What this table does:

Column	Meaning
event_id :Internal ID for each webhook event
received_at: When your system received the Fulcrum event
event_type: Whether it was create, update, delete, etc.
fulcrum_record_id: The Fulcrum record ID
payload: The full raw JSON message
processed_at	When your system processed it
processing_status: pending, processed, failed
error_message: Stores errors if something goes wrong
This table is very important because it acts like your audit log.

******Connection to SQL SERVER**********
Checking the SQL configuration manager

Enable TCP/IP Protocol.
In the left pane, expand SQL Server Network Configuration.
Click on Protocols for SQLEXPRESS.
In the right pane, right-click TCP/IP and select Enable. A warning will pop up; click OK
In the right panel of SQL Server Configuration Manager, right‑click SQL Server Browser.
Select Properties.
Set the service to start automatically
In the Properties dialog, go to the Service tab (you can see the tabs at the top: Log On, Service, Advanced).
Next to Start Mode, change it from Other (or Manual) to Automatic.
Click Apply.
SQL Server Browser is a lightweight service that only listens for incoming instance name requests.
Microsoft recommends running it under Local Service (the default) or Network Service.
Local System has far more privileges than needed – using it would violate the principle of least privilege and could create unnecessary security risks.

**Step 5: Create the Current Records Table**
This table stores the latest version of each Fulcrum record.

Run:

###USE FulcrumWarehouse;
####GO
####CREATE TABLE staging.fulcrum_records_current (
    fulcrum_record_id NVARCHAR(100) NOT NULL PRIMARY KEY,
    app_id NVARCHAR(100) NULL,
    created_at DATETIME2 NULL,
    updated_at DATETIME2 NULL,
    latitude DECIMAL(10,7) NULL,
    longitude DECIMAL(10,7) NULL,
    raw_json NVARCHAR(MAX) NOT NULL,
    last_synced_at DATETIME2 NOT NULL DEFAULT SYSUTCDATETIME(),
    is_deleted BIT NOT NULL DEFAULT 0
);
What this table does:

Column Meaning:
fulcrum_record_id:Unique Fulcrum record ID
app_id:The Fulcrum app/form ID
created_at:When the Fulcrum record was created
updated_at:When it was last updated
latitude:GPS latitude
longitude:GPS longitude
raw_json:Full latest record data
last_synced_at:When SQL Server last received it
is_deleted:Whether the record was deleted
