View PostgreSQL in Excel
=========================

This short guide will explain how you can easily bring your live postgres tables into an excel workbook. 

## Requirements 
 
 * Office 2013 (old versions may work but this guide was written for 2013)
 * Knowledge of how to download and install Microsoft Installer executables

### Step 1: Install Power Query

Power query is available for free from the Microsoft website. 
Download the same version (32 bit or 64 bit) as your office installation. 
[Learn how to check your office version](./Check Excel Version.md)

[Get Power Query](https://www.microsoft.com/en-us/download/details.aspx?id=39379)

### Step 2: Install PsqlODBC

Another free download, this time from the PostgreSQL website. Again, download the same version (32 bit or 64 bit)
as your office installation. [Get PsqlODBC](https://www.postgresql.org/ftp/odbc/versions/msi/)

### Step 3: Open Power Query

Open up a new Excel workbook. You should now see a new tab that says POWER QUERY. Open it.

### Step 4: Add A Data Source

 * Select the down arrow on From Other Sources. It is on the Get External Data panel.
 * Choose From ODBC
 * Under Data Source Name(DSN) Choose None
 * Open the advanced options and enter the connection string
 * This will look like:
 
```
Driver={PostgreSQL ANSI};Server=servername_or_ip;Port=5432;Database=mydatabase;
```

 * Enter your password and username when prompted
 * Excel will now be connected to your data tables

### Step 5: Add a table to your workbook

 * Once you are authenticated, you will be able to view your schemas and tables 
 * Choose a table to add to your workbook and click `Load`. 
 * **Note: I tested adding a Postgres view, and it did not work.**. Instead I received an error like this: 
 
```
DataSource.Error: ODBC: SUCCESS_WITH_INFO [01004] Fetched item was truencated.
```

But tables should load just fine. That's all there is to it, enjoy!

