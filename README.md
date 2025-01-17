# EasyDBMigrator ![EasyDBMigrator](https://github.com/AliDehbansiahkarbon/EasyDBMigrator/assets/5601608/964264bf-ed9e-405f-9047-673ac1428ebc)


<br />

<a href="https://www.buymeacoffee.com/adehbanr" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/default-orange.png" alt="Buy Me A Coffee" height="41" width="174"></a>
<br />

<!--img alt="MIT" src="https://img.shields.io/github/license/AliDehbansiahkarbon/EasyDBMigrator">
<img src="https://img.shields.io/github/forks/AliDehbansiahkarbon/EasyDBMigrator" alt="forks">
<img src="https://img.shields.io/github/stars/AliDehbansiahkarbon/EasyDBMigrator" alt="stars">
<img src="https://img.shields.io/github/watchers/AliDehbansiahkarbon/EasyDBMigrator" alt="watchers">
<br />
<a href="https://github.com/AliDehbansiahkarbon/EasyDBMigrator/issues"><img src="https://img.shields.io/github/issues-closed/AliDehbansiahkarbon/EasyDBMigrator" alt="issues"></a>
<a href="https://github.com/AliDehbansiahkarbon/EasyDBMigrator/pulls"><img src="https://img.shields.io/github/issues-pr-closed/AliDehbansiahkarbon/EasyDBMigrator" alt="pulls"></a>
<img src="https://img.shields.io/github/last-commit/AliDehbansiahkarbon/ChatGPTWizard.svg" alt="last-commit" -->


## EasyDbMigrator is a database migration library designed for Delphi. It simplifies database evolution and is available in both 32-Bit and 64-Bit versions.
## Migrations are structured objects designed to alter your database schema. They provide an alternative to creating numerous SQL scripts that would require manual execution by every developer involved.
When dealing with multiple databases, such as the developer's local database, test database, and production database, migrations are a helpful solution for evolving a database schema. These changes to the schema are recorded in Delphi classes, which can then be committed to a version control system.

# Covered databases and available examples:

|Name | Simple | Advanced | ORM | LargeScript Execution|
|---|---|---|---|---|
| **Microsoft SQL SERVER** | ✅ | ✅ | ✅ | ✅ |
| **MySQL** | ✅ | ✅ | ✅ | ✅ |
| **MariaDB** | ✅ | ✅ | ✅ | ✅ |
| **PostgreSQL** | ✅ | ✅ | - | - |
| **Oracle** | ✅ | ✅ | - | - |
| **Firebird** | ✅ | ✅ | - | - |

# Supported Delphi Versions 
```delphi
Delphi XE5
Delphi XE6
Delphi XE7
Delphi XE8
Delphi 10 Seattle
Delphi 10.1 Berlin
Delphi 10.2 Tokyo
Delphi 10.3 Rio
Delphi 10.4 Sydney
Delphi 11.0 Alexandria
```

# How it works?
To use the library, simply incorporate the units into your projects, implement migrations, and execute the migrator. It's that simple.

## (If you want to see some demo videos, please visit [here](https://www.youtube.com/playlist?list=PLsToBC7EKBNSkD-mwMN18TQbJ-lzistS7) or click on the below image👇👇)
<a href="https://www.youtube.com/playlist?list=PLsToBC7EKBNSkD-mwMN18TQbJ-lzistS7" target="_blank"><img src="https://github-production-user-asset-6210df.s3.amazonaws.com/5601608/259202139-98eda93e-6e60-4469-b1ab-d538e371cfd3.png" width = "700" height = "400" /></a>



# Can it be utilized with other databases?
Certainly! You can seamlessly integrate it with your environment. Kindly refer to the integration section for more details.

# How to use it?
To get started quickly, please take a look at the sample codes provided. These examples showcase how to use the library and include extra details.<br>
To use this library, you must add the library paths to your project's search path or Delphi's library path.
It is necessary to include lines that are similar to the following: 
```delphi
..\..\..\lib\Logger;
..\..\..\lib\ConnectionManagers;
..\..\..\lib\Core;
..\..\..\lib\Runners;
..\..\..\lib\ORM
```

# There are various kinds of samples available.

- <ins>**Simple**</ins>: Suitable for **small** projects (using on-demand classes with anonymous methods).
- <ins>**Advanced**</ins>: Suitable for **large** projects (using versioned classes with attributes).
  Instead of creating some on-demand classes you can create one unit per entity and implement versioned classes.
  
- <ins>**ORM**</ins>: Suitable for both small and large projects.
- <ins>**Large Script**</ins>: This is a script executor that allows you to use your existing old scripts as a starting point and continue working with this library from now on.

# Dependencies
To work with certain database engines such as Mysql(libmysql32.dll) or PostgreSQL(libpq.dll), you will need to have the appropriate Dlls. There are no other prerequisite libraries or components required. It is important to ensure that you provide the correct Dll or class library depending on the specific version of the database engine installed in your environment. Please note that this library does not include the necessary DLLs, but rather provides a template for each database engine.

- ## Note:
A condition definition has been added to provide additional features such as additional overridden constructors and functions.
This is to prevent compiler prompts/hints for unused functions, if you want access to all functionalities, you can use "FullOptions" as a conditional definition of your project.

---
# I have provided some samples and templates for your reference.👇
- # Simple

<details>
<summary>
  🟢 SQL SERVER Sample 
</summary>
<br>
  - Initializing

```delphi
uses
  EasyDB.Core,
  EasyDB.Migration,
  EasyDB.MSSQLRunner,
  EasyDB.Logger;

var
  Runner: TSQLRunner;
  ConnectionParams: TConnectionParams;
begin

  with LvConnectionParams do // Could be loaded from ini, registry, or somewhere else.
  begin
    Server := '127.0.0.1'; // SQL Server address
    LoginTimeout := 30000;
    Username := 'sa';
    Pass := '1';
    DatabaseName := 'Library';
    Schema := 'dbo'; //Optional
  end;

  {Use this line if you need a local log}
  TLogger.Instance.ConfigLocal(True, 'C:\Temp\EasyDBLog.txt').OnLog := OnLog; // Logger must be configured before creating the Runner.

  {Use this line if you don't need a local log}
  // TLogger.Instance.OnLog := OnLog;

  Runner := TSQLRunner.Create(LvConnectionParams);
  Runner.AddConfig.LogAllExecutions(True).UseInternalThread(True).SetProgressbar(pbTotal).RollBackAllByAnyError(True); //each part This line is Optional
end
```

- Add migrations
```delphi

Runner.MigrationList.Add(TMigration.Create('TbUsers', 202301010001, 'Alex', 'Create table Users, Task Number #2701',
  procedure
  var sql: string;
  begin
    sql := 'If Not Exists( Select * From sysobjects Where Name = ''TbUsers'' And xtype = ''U'') ' + #10
     + '    Create Table TbUsers( ' + #10
     + '    	ID Int Primary key Identity(1, 1) Not null, ' + #10
     + '    	UserName Nvarchar(100), ' + #10
     + '    	Pass Nvarchar(50) ' + #10
     + '    );';

    Runner.SQLConnection.ExecuteAdHocQuery(sql);
  end,
  procedure
  begin
    Runner.SQLConnection.ExecuteAdHocQuery('DROP TABLE TbUsers');
  end
  ));

  //============================================
  Runner.MigrationList.Add(TMigration.Create('TbUsers', 202301010002, 'Ali', 'Task number #2701',
  procedure
  begin
    Runner.SQLConnection.ExecuteAdHocQuery('ALTER TABLE TbUsers ADD NewField2 VARCHAR(50)');
  end,
  procedure
  begin
    Runner.SQLConnection.ExecuteAdHocQuery('ALTER TABLE TbUsers DROP COLUMN NewField2');
  end
  ));

  //============================================

  Runner.MigrationList.Add(TMigration.Create('TbUsers', 202301010003, 'Ali', 'Task number #2702',
  procedure
  begin
    Runner.SQLConnection.ExecuteAdHocQuery('ALTER TABLE TbUsers ADD NewField3 INT');
  end,
  procedure
  begin
    Runner.SQLConnection.ExecuteAdHocQuery('ALTER TABLE TbUsers DROP COLUMN NewField3');
  end
  ));

  //============================================

  Runner.MigrationList.Add(TMigration.Create('TbCustomers', 202301010003, 'Alex', 'Task number #2702',
  procedure
  var sql: string;
  begin
    sql := 'If Not Exists( Select * From sysobjects Where Name = ''TbCustomers'' And xtype = ''U'') ' + #10
     + '    Create Table TbCustomers( ' + #10
     + '    	ID Int Primary key Identity(1, 1) Not null, ' + #10
     + '    	Name Nvarchar(100), ' + #10
     + '    	Family Nvarchar(50) ' + #10
     + '    );';
    Runner.SQLConnection.ExecuteAdHocQuery(sql);
  end,
  procedure
  begin
    Runner.SQLConnection.ExecuteAdHocQuery('DROP TABLE TbCustomers');
  end
  ));

//...
//Add other migrations here
//...

```

- Run the Migrator
```delphi
  Runner.UpgradeDatabase; // Do upgrade
  // Or 
  Runner.DowngradeDatabase(202301010001); // Do downgrade to a specific version.
  //This version and lower versions of the database will remain and any version above this will be reverted.
```    
</details>

<details>
<summary>
  🟠 MySQL Sample
</summary>
 <br> 
  - Initializing
  
 ```delphi

uses
  EasyDB.Core,
  EasyDB.Logger,
  EasyDB.Migration,
  EasyDB.MySQLRunner;

var
  LvConnectionParams: TMySqlConnectionParams;
begin
  with LvConnectionParams do // Could be loaded from ini, registry, or somewhere else.
  begin
    Server := '127.0.0.1';
    LoginTimeout := 30000;
    Port := 3306;
    Username := 'ali';
    Pass := 'Admin123!@#';
    Schema := 'Library';
  end;

  Runner := TMySQLRunner.Create(LvConnectionParams);
  Runner.Config
    .LogAllExecutions(True) // Optional
    .UseInternalThread(True) //Optional - executes asynchronously and doesn't block the screen
    .SetProgressbar(pbTotal); //Optional

  {Use this line if you don't need local log}
  Runner.AddLogger.OnLog := OnLog;

  {Use this line if you need local log}
  //Runner.AddLogger.ConfigLocal(True, 'C:\Temp\EasyDBLog.txt').OnLog := OnLog;
```

  - Add migrations
  ```delphi
  //Modern way
  Runner.Clear
  .Add(TUsersMgr_202301010001.Create)
  .Add(TUsersMgr_202301010002.Create)
  .Add(TUsersMgr_202301010003.Create)
  .Add(TCustomersMgr_202301010005.Create)
  .Add(TCustomersMgr_202301010010.Create)
  .Add(TInvoicesMgr_202301010005.Create)
  .Add(TInvoicesMgr_202301010010.Create);

  // Classic Way
{
  Runner.Clear;
  Runner.MigrationList.Add(TUsersMgr_202301010001.Create);
  Runner.MigrationList.Add(TUsersMgr_202301010002.Create);
  Runner.MigrationList.Add(TUsersMgr_202301010003.Create);

  Runner.MigrationList.Add(TCustomersMgr_202301010005.Create);
  Runner.MigrationList.Add(TCustomersMgr_202301010010.Create);

  Runner.MigrationList.Add(TInvoicesMgr_202301010005.Create);
  Runner.MigrationList.Add(TInvoicesMgr_202301010010.Create);
}
```  
 
- Run the Migrator
```delphi
  Runner.UpgradeDatabase; // Do upgrade
  //OR
  Runner.DowngradeDatabase(202301010001); // Do downgrade to a specific version.
  //This version and lower versions of the database will remain and any version above this will be restored.
``` 
</details>

<details>
<summary>
  🟣 MariaDB sample
</summary>
<br>
  
- Initializing
```delphi
uses
  EasyDB.Core,
  EasyDB.Logger,
  EasyDB.Migration,
  EasyDB.MariaDBRunner;

var
  LvConnectionParams: TMariaDBConnectionParams;
begin
  with LvConnectionParams do // Could be loaded from ini, registry or somewhere else.
  begin
    Server := '127.0.0.1';
    LoginTimeout := 30000;
    Port := 3306;
    UserName := 'ali';
    Pass := 'Admin123!@#';
    Schema := 'Library';
  end;

  Runner := TMariaDBRunner.Create(LvConnectionParams);
  Runner.Config
    .LogAllExecutions(True) // Optional
    .UseInternalThread(True) //Optional - executes asynchronously and doesn't block the screen
    .SetProgressbar(pbTotal); //Optional

  {Use this line if you don't need local log}
  Runner.AddLogger.OnLog := OnLog;

  {Use this line if you need local log}
  //Runner.AddLogger.ConfigLocal(True, 'C:\Temp\EasyDBLog.txt').OnLog := OnLog;
end;
```
- Add migrations
```delphi
  Runner.Clear;
  Runner.Add(TMigration.Create('TbUsers', 202301010001, 'Ali', 'Create table Users, #2701',
  procedure
  var sql: string;
  begin
    sql := 'CREATE TABLE IF NOT EXISTS TbUsers ( ' + #10
     + '    ID INT NOT NULL PRIMARY KEY, ' + #10
     + '    UserName NVARCHAR(100), ' + #10
     + '    Pass NVARCHAR(100) ' + #10
     + '    );';

    Runner.MySQL.ExecuteAdHocQuery(sql);
  end,
  procedure
  begin
    Runner.MySQL.ExecuteAdHocQuery('DROP TABLE TbUsers');
  end
  ));
  //============================================
  Runner.Add(TMigration.Create('TbUsers', 202301010002, 'Ali', 'Task number #2701',
  procedure
  begin
    Runner.MySQL.ExecuteAdHocQuery('ALTER TABLE TbUsers ADD NewField2 VARCHAR(50)');
  end,
  procedure
  begin
    Runner.MySQL.ExecuteAdHocQuery('ALTER TABLE TbUsers DROP COLUMN NewField2');
  end
  ));
  //============================================
  Runner.Add(TMigration.Create('TbUsers', 202301010003, 'Ali', 'Task number #2702',
  procedure
  begin
    Runner.MySQL.ExecuteAdHocQuery('ALTER TABLE TbUsers ADD NewField3 INT');
  end,
  procedure
  begin
    Runner.MySQL.ExecuteAdHocQuery('ALTER TABLE TbUsers DROP COLUMN NewField3');
  end
  ));
  //============================================
  Runner.Add(TMigration.Create('TbCustomers', 202301010003, 'Alex', 'Task number #2702',
  procedure
  var sql: string;
  begin
    sql := 'CREATE TABLE IF NOT EXISTS TbCustomers ( ' + #10
     + '    ID INT NOT NULL PRIMARY KEY, ' + #10
     + '    Name NVARCHAR(100), ' + #10
     + '    Family NVARCHAR(100) ' + #10
     + '    );';

    Runner.MySQL.ExecuteAdHocQuery(sql);
  end,
  procedure
  begin
    Runner.MySQL.ExecuteAdHocQuery('DROP TABLE TbCustomers');
  end
  ));
```
- Run the Migrator
```delphi
  Runner.UpgradeDatabase; // Do upgrade
  //OR
  Runner.DowngradeDatabase(202301010001); // Do downgrade to a specific version.
  //This version and lower versions of the database will remain and any version above this will be restored.
```

</details>

<details>
<summary>
  🔵 PostgreSQL sample
</summary>
<br>
- Initializing
  
```delphi
uses
  EasyDB.Core,
  EasyDB.Migration,
  EasyDB.PostgreSQLRunner,
  EasyDB.Logger;

  var
  LvConnectionParams: TPgConnectionParams;
begin
  with LvConnectionParams do // Could be loaded from ini, registry or somewhere else.
  begin
    Server := '192.168.212.1';
    LoginTimeout := 30000;
    Port := 5432;
    UserName := 'postgres';
    Pass := 'Admin123!@#';
    DatabaseName := 'Library';
    Schema := 'public';
  end;

  Runner := TPgRunner.Create(LvConnectionParams);
  Runner.Config
    .LogAllExecutions(True) // Optional
    .UseInternalThread(True) //Optional - executes asynchronously and doesn't block the screen
    .SetProgressbar(pbTotal); //Optional

  {Use this line if you don't need local log}
  Runner.AddLogger.OnLog := OnLog;

  {Use this line if you need local log}
  //Runner.AddLogger.ConfigLocal(True, 'C:\Temp\EasyDBLog.txt').OnLog := OnLog;
end;
```  
- Add migrations

```delphi
  Runner.Clear;
  Runner.Add(TMigration.Create('TbUsers', 202301010001, 'Ali', 'Create table Users, #2701',
  procedure
  var sql: string;
  begin
    sql := 'CREATE TABLE IF NOT EXISTS public.tbusers' + #10 +
    '('+ #10 +
    'id integer NOT NULL DEFAULT nextval(''tbusers_id_seq''::regclass),' + #10 +
    'username character varying(100) COLLATE pg_catalog."default",' + #10 +
    'pass character varying(50) COLLATE pg_catalog."default",' + #10 +
    'CONSTRAINT tbusers_pkey PRIMARY KEY (id)' + #10 +
    ');';

    Runner.PG.ExecuteAdHocQuery(sql);
  end,
  procedure
  begin
    Runner.PG.ExecuteAdHocQuery('DROP TABLE IF EXISTS public.tbusers;');
  end
  ));
  //============================================
  Runner.Add(TMigration.Create('TbUsers', 202301010002, 'Ali', 'Task number #2701',
  procedure
  begin
    Runner.PG.ExecuteAdHocQuery('ALTER TABLE TbUsers ADD NewField2 VARCHAR(50)');
  end,
  procedure
  begin
    Runner.PG.ExecuteAdHocQuery('ALTER TABLE TbUsers DROP COLUMN NewField2');
  end
  ));
  //============================================
  Runner.Add(TMigration.Create('TbUsers', 202301010003, 'Ali', 'Task number #2702',
  procedure
  begin
    Runner.PG.ExecuteAdHocQuery('ALTER TABLE TbUsers ADD NewField3 INT');
  end,
  procedure
  begin
    Runner.PG.ExecuteAdHocQuery('ALTER TABLE TbUsers DROP COLUMN NewField3');
  end
  ));
  //============================================
  Runner.Add(TMigration.Create('TbCustomers', 202301010003, 'Alex', 'Task number #2702',
  procedure
  var sql: string;
  begin
    sql := 'CREATE TABLE IF NOT EXISTS public.tbcustomers' + #10 +
    '(' + #10 +
    'id integer NOT NULL DEFAULT nextval(''tbcustomers_id_seq''::regclass),' + #10 +
    'name character varying(100) COLLATE pg_catalog."default",' + #10 +
    'family character varying(50) COLLATE pg_catalog."default",' + #10 +
    'phone character varying(10) COLLATE pg_catalog."default",' + #10 +
    'CONSTRAINT tbcustomers_pkey PRIMARY KEY (id)' + #10 +
    ')';
    Runner.PG.ExecuteAdHocQuery(sql);
  end,
  procedure
  begin
    Runner.PG.ExecuteAdHocQuery('DROP TABLE IF EXISTS public.tbcustomers');
  end
  ));
  //============================================
  Runner.Add(TMigration.Create('TbCustomers', 202301010004, 'Tom', 'Task number #2703',
  procedure
  var sql: string;
  begin
    sql := 'CREATE TABLE IF NOT EXISTS public.tbinvoices' + #10 +
    '(' + #10 +
    'id integer NOT NULL DEFAULT nextval(''tbinvoices_id_seq''::regclass),' + #10 +
    'invoiceid integer,' + #10 +
    'customerid integer,' + #10 +
    'invoicedate timestamp without time zone,' + #10 +
    'totalamount numeric(10,2),' + #10 +
    'CONSTRAINT tbinvoices_pkey PRIMARY KEY (id)' + #10 +
    ')';
    Runner.PG.ExecuteAdHocQuery(sql);
  end,
  procedure
  begin
    Runner.PG.ExecuteAdHocQuery('DROP TABLE IF EXISTS public.tbinvoices');
  end
  ));
```
- Run the Migrator
```delphi
  Runner.UpgradeDatabase; // Do upgrade
  //OR
  Runner.DowngradeDatabase(202301010001); // Do downgrade to a specific version.
  //This version and lower versions of the database will remain and any version above this will be restored.
```
</details>

<details>
<summary>
  🔴 Oracle Sample 
</summary>
<br>  
- Initializing
  
```delphi
uses
  EasyDB.Core,
  EasyDB.Migration,
  EasyDB.OracleRunner,
  EasyDB.Logger;

var
  LvConnectionParams: TOracleConnectionParams;
begin
  with LvConnectionParams do // Could be loaded from ini, registry or somewhere else.
  begin
    Server := '127.0.0.1';
    LoginTimeout := 30000;
    UserName := 'admin';
    Pass := '123';
    DatabaseName := 'Library';
  end;

  Runner := TOracleRunner.Create(LvConnectionParams);
  Runner.Config
    .LogAllExecutions(True)// Optional
    .UseInternalThread(True)// Optional - executes asynchronously and doesn't block the screen
    .SetProgressbar(pbTotal);// Optional

  {Use this line if you don't need local log}
  Runner.AddLogger.OnLog := OnLog;

  {Use this line if you need local log}
  //Runner.AddLogger.ConfigLocal(True, 'C:\Temp\EasyDBLog.txt').OnLog := OnLog;
end;
```

- Add migrations

```delphi
  Runner.Clear;
  Runner.Add(TMigration.Create('TbUsers', 202301010001, 'Ali', 'Create table Users, #2701',
  procedure
  var sql: string;
  begin
    sql := 'CREATE TABLE TbUsers( ' + #10
     + '    ID INT NOT NULL PRIMARY KEY, ' + #10
     + '    UserName NVARCHAR2(100), ' + #10
     + '    Pass NVARCHAR2(100) ' + #10
     + '    );';

    Runner.Oracle.ExecuteAdHocQuery(sql);
  end,
  procedure
  begin
    Runner.Oracle.ExecuteAdHocQuery('DROP TABLE TbUsers');
  end
  ));
  //============================================
  Runner.Add(TMigration.Create('TbUsers', 202301010002, 'Ali', 'Task number #2701',
  procedure
  begin
    Runner.Oracle.ExecuteAdHocQuery('ALTER TABLE TbUsers ADD NewField2 VARCHAR(50)');
  end,
  procedure
  begin
    Runner.Oracle.ExecuteAdHocQuery('ALTER TABLE TbUsers DROP COLUMN NewField2');
  end
  ));
  //============================================
  Runner.Add(TMigration.Create('TbUsers', 202301010003, 'Ali', 'Task number #2702',
  procedure
  begin
    Runner.Oracle.ExecuteAdHocQuery('ALTER TABLE TbUsers ADD NewField3 INT');
  end,
  procedure
  begin
    Runner.Oracle.ExecuteAdHocQuery('ALTER TABLE TbUsers DROP COLUMN NewField3');
  end
  ));
  //============================================
  Runner.Add(TMigration.Create('TbCustomers', 202301010003, 'Alex', 'Task number #2702',
  procedure
  var sql: string;
  begin
    sql := 'CREATE TABLE TbCustomers( ' + #10
     + '    ID INT NOT NULL PRIMARY KEY, ' + #10
     + '    Name NVARCHAR2(100), ' + #10
     + '    Family NVARCHAR2(100) ' + #10
     + '    );';

    Runner.Oracle.ExecuteAdHocQuery(sql);
  end,
  procedure
  begin
    Runner.Oracle.ExecuteAdHocQuery('DROP TABLE TbCustomers');
  end
  ));
```

- Run the Migrator
```delphi
  Runner.UpgradeDatabase; // Do upgrade
  //OR
  Runner.DowngradeDatabase(202301010001); // Do downgrade to a specific version.
  //This version and lower versions of the database will remain and any version above this will be restored.
```

</details>


- # Advanced

<details>
  <summary>
   🟢 SQL SERVER Sample
  </summary>
  <br>
  - Initializing

```delphi
uses
  EasyDB.Core,
  EasyDB.Migration,
  EasyDB.MSSQLRunner,
  EasyDB.Logger;

var
  LvConnectionParams: TSqlConnectionParams;
begin
  with LvConnectionParams do // The information can be sourced from an ini file, registry or other location.
  begin
    Server := '192.168.212.1';
    LoginTimeout := 30000;
    UserName := 'sa';
    Pass := '1';
    DatabaseName := 'Library';
    Schema := 'dbo';
  end;

  Runner := TSQLRunner.Create(LvConnectionParams);
  Runner.Config
    .LogAllExecutions(True)// Optional
    .UseInternalThread(True)// Optional - executes asynchronously and doesn't block the screen
    .SetProgressbar(pbTotal)// Optional
    .DelayedExecution(500); //Just for test

  {Use this line if you don't need local log}
  Runner.AddLogger.OnLog := OnLog;

  {Use this line if you need local log}
  //Runner.AddLogger.ConfigLocal(True, 'C:\Temp\EasyDBLog.txt').OnLog := OnLog;
end;
```  
- Define migrations in diffrent place(unit)

```delphi
uses
  EasyDB.Core,
  EasyDB.ConnectionManager.SQL,
  EasyDB.MigrationX,
  EasyDB.MSSQLRunner,
  EasyDB.Logger;

type

  [TCustomMigrationAttribute('TbUsers', 202301010001, 'Created users table', 'Alex')]
  TUsersMgr_202301010001 = class(TMigrationX)
  public
    procedure Upgrade; override;
    procedure Downgrade; override;
  end;

  [TCustomMigrationAttribute('TbUsers', 202301010002, 'Added newfielad1', 'Alex')]
  TUsersMgr_202301010002 = class(TMigrationX)
  public
    procedure Upgrade; override;
    procedure Downgrade; override;
  end;

  [TCustomMigrationAttribute('TbUsers', 202301010003, 'Added newfielad2', 'Alex')]
  TUsersMgr_202301010003 = class(TMigrationX)
  public
    procedure Upgrade; override;
    procedure Downgrade; override;
  end;

implementation

{ TUsersMgr_202301010001 }
procedure TUsersMgr_202301010001.Downgrade;
begin
  try
    SQL.ExecuteAdHocQuery('Drop Table TbUsers');
  except on E: Exception do
    Logger.Log(atDowngrade, E.Message, AttribEntityName, AttribVersion);
  end;
end;

procedure TUsersMgr_202301010001.Upgrade;
var
  LvScript: string;
begin
  LvScript := 'If Not Exists( Select * From sysobjects Where Name = ''TbUsers'' And xtype = ''U'') ' + #10
            + ' Create Table TbUsers( ' + #10
            + ' ID Int Primary key Identity(1, 1) Not null, ' + #10
            + ' UserName Nvarchar(100), ' + #10
            + ' Pass Nvarchar(50) ' + #10
            + ' );';
  try
    SQL.ExecuteAdHocQuery(LvScript);
  except on E: Exception do
    Logger.Log(atUpgrade, E.Message, AttribEntityName, AttribVersion);
  end;
end;

{ TUsersMgr_202301010002 }
procedure TUsersMgr_202301010002.Downgrade;
var
  LvScript: string;
begin
  try
    SQL.ExecuteAdHocQuery('Alter table TbUsers Drop Column CreatedDate');
  except on E: Exception do
    Logger.Log(atUpgrade, E.Message, AttribEntityName, AttribVersion);
  end;
end;

procedure TUsersMgr_202301010002.Upgrade;
begin
  try
    SQL.ExecuteAdHocQuery('Alter table TbUsers Add CreatedDate Datetime');
  except on E: Exception do
    Logger.Log(atUpgrade, E.Message, AttribEntityName, AttribVersion);
  end;
end;

{ TUsersMgr_202301010003 }

procedure TUsersMgr_202301010003.Downgrade;
begin
  try
    SQL.ExecuteAdHocQuery('Alter table TbUsers Drop Column ImageLink');
  except on E: Exception do
    Logger.Log(atUpgrade, E.Message, AttribEntityName, AttribVersion);
  end;
end;

procedure TUsersMgr_202301010003.Upgrade;
begin
  try
    SQL.ExecuteAdHocQuery('Alter table TbUsers Add ImageLink Varchar(500)');
  except on E: Exception do
    Logger.Log(atUpgrade, E.Message, AttribEntityName, AttribVersion);
  end;
end;
```

- Add migrations
  
```delphi
  //Modern way
  Runner.Clear
  .Add(TUsersMgr_202301010001.Create)
  .Add(TUsersMgr_202301010002.Create)
  .Add(TUsersMgr_202301010003.Create)
  .Add(TCustomersMgr_202301010005.Create)
  .Add(TCustomersMgr_202301010010.Create)
  .Add(TInvoicesMgr_202301010005.Create)
  .Add(TInvoicesMgr_202301010010.Create);

  // Classic Way
{
  Runner.Clear;
  Runner.MigrationList.Add(TUsersMgr_202301010001.Create);
  Runner.MigrationList.Add(TUsersMgr_202301010002.Create);
  Runner.MigrationList.Add(TUsersMgr_202301010003.Create);

  Runner.MigrationList.Add(TCustomersMgr_202301010005.Create);
  Runner.MigrationList.Add(TCustomersMgr_202301010010.Create);

  Runner.MigrationList.Add(TInvoicesMgr_202301010005.Create);
  Runner.MigrationList.Add(TInvoicesMgr_202301010010.Create);
}
```

- Run the Migrator exactly like the simple mode.
```delphi
  Runner.UpgradeDatabase; // Do upgrade
  //OR
  Runner.DowngradeDatabase(202301010001); // Do downgrade to a specific version.
  //This version and lower versions of the database will remain and any version above this will be restored.
```
</details>

<details>
  <summary>
   🟠 MySQL Sample
  </summary>
  <br>
  - Initializing
   
```delphi
  uses
    EasyDB.Core,
    EasyDB.ConnectionManager.MySQL,
    EasyDB.MigrationX,
    EasyDB.MySQLRunner,
    EasyDB.Logger;

var
  LvConnectionParams: TMySqlConnectionParams;
begin
  with LvConnectionParams do // Could be loaded from ini, registry or somewhere else.
  begin
    Server := '127.0.0.1';
    LoginTimeout := 30000;
    Port := 3306;
    UserName := 'ali';
    Pass := 'Admin123!@#';
    Schema := 'Library';
  end;

  Runner := TmySQLRunner.Create(LvConnectionParams);
  Runner.Config
    .LogAllExecutions(True) // Optional
    .UseInternalThread(True) //Optional
    .SetProgressbar(pbTotal); //Optional

  {Use this line if you don't need local log}
  Runner.AddLogger.OnLog := OnLog;

  {Use this line if you need local log}
  //Runner.AddLogger.ConfigLocal(True, 'C:\Temp\EasyDBLog.txt').OnLog := OnLog;
 ```
- Define migrations in diffrent place(unit)
```delphi
uses
  System.SysUtils,

  EasyDB.Core,
  EasyDB.MigrationX,
  EasyDB.Attribute,
  UHelper;

type

  [TCustomMigrationAttribute('TbUsers', 202301010001, 'Created users table', 'Alex')]
  TUsersMgr_202301010001 = class(TMigrationX)
  public
    procedure Upgrade; override;
    procedure Downgrade; override;
  end;

  [TCustomMigrationAttribute('TbUsers', 202301010002, 'Added newfielad1', 'Alex')]
  TUsersMgr_202301010002 = class(TMigrationX)
  public
    procedure Upgrade; override;
    procedure Downgrade; override;
  end;

  [TCustomMigrationAttribute('TbUsers', 202301010003, 'Added newfielad2', 'Alex')]
  TUsersMgr_202301010003 = class(TMigrationX)
  public
    procedure Upgrade; override;
    procedure Downgrade; override;
  end;

implementation

{ TUsersMgr_202301010001 }
procedure TUsersMgr_202301010001.Downgrade;
begin
  try
    MySQL.ExecuteAdHocQuery('Drop Table TbUsers');
  except on E: Exception do
    Logger.Log(atDowngrade, E.Message, AttribEntityName, AttribVersion);
  end;
end;

procedure TUsersMgr_202301010001.Upgrade;
var
  LvScript: string;
begin
  LvScript := 'CREATE TABLE IF NOT EXISTS TbUsers ( ' + #10
            + '    ID INT NOT NULL PRIMARY KEY, ' + #10
            + '    UserName NVARCHAR(100), ' + #10
            + '    Pass NVARCHAR(100) ' + #10
            + '    );';
  try
    MySQL.ExecuteAdHocQuery(LvScript);
  except on E: Exception do
    Logger.Log(atUpgrade, E.Message, AttribEntityName, AttribVersion);
  end;
end;

{ TUsersMgr_202301010002 }
procedure TUsersMgr_202301010002.Downgrade;
begin
  try
    MySQL.ExecuteAdHocQuery('Alter table TbUsers Drop Column CreatedDate');
  except on E: Exception do
    Logger.Log(atUpgrade, E.Message, AttribEntityName, AttribVersion);
  end;
end;

procedure TUsersMgr_202301010002.Upgrade;
begin
  try
    MySQL.ExecuteAdHocQuery('Alter table TbUsers Add CreatedDate Datetime');
  except on E: Exception do
    Logger.Log(atUpgrade, E.Message, AttribEntityName, AttribVersion);
  end;
end;

{ TUsersMgr_202301010003 }

procedure TUsersMgr_202301010003.Downgrade;
begin
  try
    MySQL.ExecuteAdHocQuery('Alter table TbUsers Drop Column ImageLink');
  except on E: Exception do
    Logger.Log(atUpgrade, E.Message, AttribEntityName, AttribVersion);
  end;
end;

procedure TUsersMgr_202301010003.Upgrade;
begin
  try
    MySQL.ExecuteAdHocQuery('Alter table TbUsers Add ImageLink Varchar(500)');
  except on E: Exception do
    Logger.Log(atUpgrade, E.Message, AttribEntityName, AttribVersion);
  end;
end;
```

- Add migrations

```delphi
  //Modern way
  Runner.Clear
  .Add(TUsersMgr_202301010001.Create)
  .Add(TUsersMgr_202301010002.Create)
  .Add(TUsersMgr_202301010003.Create)
  .Add(TCustomersMgr_202301010005.Create)
  .Add(TCustomersMgr_202301010010.Create)
  .Add(TInvoicesMgr_202301010005.Create)
  .Add(TInvoicesMgr_202301010010.Create);

  // Classic Way
{
  Runner.Clear;
  Runner.MigrationList.Add(TUsersMgr_202301010001.Create);
  Runner.MigrationList.Add(TUsersMgr_202301010002.Create);
  Runner.MigrationList.Add(TUsersMgr_202301010003.Create);

  Runner.MigrationList.Add(TCustomersMgr_202301010005.Create);
  Runner.MigrationList.Add(TCustomersMgr_202301010010.Create);

  Runner.MigrationList.Add(TInvoicesMgr_202301010005.Create);
  Runner.MigrationList.Add(TInvoicesMgr_202301010010.Create);
}
```
 
- Run the Migrator

```delphi
  Runner.UpgradeDatabase; // Do upgrade
  //OR
  Runner.DowngradeDatabase(202301010001); // Do downgrade to a specific version.
  //This version and lower versions of the database will remain and any version above this will be restored.
```  
</details>

<details>
  <summary>
   🟣 MariaDb Sample
  </summary>
<br>
  
- Initializing
```delphi
uses
  EasyDB.Core,
  EasyDB.ConnectionManager.MariaDB,
  EasyDB.MigrationX, // Do not use "EasyDB.Migration.Base" here if you prefer to use Attributes.
  EasyDB.MariaDBRunner,
  EasyDB.Logger;

var
  LvConnectionParams: TMariaDBConnectionParams;
begin
  with LvConnectionParams do // Could be loaded from ini, registry or somewhere else.
  begin
    Server := '127.0.0.1';
    LoginTimeout := 30000;
    Port := 3306;
    UserName := 'ali';
    Pass := 'Admin123!@#';
    Schema := 'Library';
  end;

  Runner := TMariaDBRunner.Create(LvConnectionParams);
  Runner.Config
    .LogAllExecutions(True) // Optional
    .UseInternalThread(True) //Optional
    .SetProgressbar(pbTotal); //Optional

  {Use this line if you don't need local log}
  Runner.AddLogger.OnLog := OnLog;

  {Use this line if you need local log}
  //Runner.AddLogger.ConfigLocal(True, 'C:\Temp\EasyDBLog.txt').OnLog := OnLog;
end;
```
- Define migrations in diffrent place(unit)
```delphi
uses
  System.SysUtils,
  EasyDB.Core,
  EasyDB.MigrationX,
  EasyDB.Attribute,
  UHelper;

type

  [TCustomMigrationAttribute('TbUsers', 202301010001, 'Created users table', 'Alex')]
  TUsersMgr_202301010001 = class(TMigrationX)
  public
    procedure Upgrade; override;
    procedure Downgrade; override;
  end;

  [TCustomMigrationAttribute('TbUsers', 202301010002, 'Added newfielad1', 'Alex')]
  TUsersMgr_202301010002 = class(TMigrationX)
  public
    procedure Upgrade; override;
    procedure Downgrade; override;
  end;

  [TCustomMigrationAttribute('TbUsers', 202301010003, 'Added newfielad2', 'Alex')]
  TUsersMgr_202301010003 = class(TMigrationX)
  public
    procedure Upgrade; override;
    procedure Downgrade; override;
  end;

implementation

{ TUsersMgr_202301010001 }
procedure TUsersMgr_202301010001.Downgrade;
begin
  try
    MySQL.ExecuteAdHocQuery('Drop Table TbUsers');
  except on E: Exception do
    Logger.Log(atDowngrade, E.Message, AttribEntityName, AttribVersion);
  end;
end;

procedure TUsersMgr_202301010001.Upgrade;
var
  LvScript: string;
begin
  LvScript := 'CREATE TABLE IF NOT EXISTS TbUsers ( ' + #10
            + '    ID INT NOT NULL PRIMARY KEY, ' + #10
            + '    UserName NVARCHAR(100), ' + #10
            + '    Pass NVARCHAR(100) ' + #10
            + '    );';
  try
    MySQL.ExecuteAdHocQuery(LvScript);
  except on E: Exception do
    Logger.Log(atUpgrade, E.Message, AttribEntityName, AttribVersion);
  end;
end;

{ TUsersMgr_202301010002 }
procedure TUsersMgr_202301010002.Downgrade;
begin
  try
    MySQL.ExecuteAdHocQuery('Alter table TbUsers Drop Column CreatedDate');
  except on E: Exception do
    Logger.Log(atUpgrade, E.Message, AttribEntityName, AttribVersion);
  end;
end;

procedure TUsersMgr_202301010002.Upgrade;
begin
  try
    MySQL.ExecuteAdHocQuery('Alter table TbUsers Add CreatedDate Datetime');
  except on E: Exception do
    Logger.Log(atUpgrade, E.Message, AttribEntityName, AttribVersion);
  end;
end;

{ TUsersMgr_202301010003 }

procedure TUsersMgr_202301010003.Downgrade;
begin
  try
    MySQL.ExecuteAdHocQuery('Alter table TbUsers Drop Column ImageLink');
  except on E: Exception do
    Logger.Log(atUpgrade, E.Message, AttribEntityName, AttribVersion);
  end;
end;

procedure TUsersMgr_202301010003.Upgrade;
begin
  try
    MySQL.ExecuteAdHocQuery('Alter table TbUsers Add ImageLink Varchar(500)');
  except on E: Exception do
    Logger.Log(atUpgrade, E.Message, AttribEntityName, AttribVersion);
  end;
end;
```

- Add migrations
```delphi
  //Modern way
  Runner.Clear
  .Add(TUsersMgr_202301010001.Create)
  .Add(TUsersMgr_202301010002.Create)
  .Add(TUsersMgr_202301010003.Create)
  .Add(TCustomersMgr_202301010005.Create)
  .Add(TCustomersMgr_202301010010.Create)
  .Add(TInvoicesMgr_202301010005.Create)
  .Add(TInvoicesMgr_202301010010.Create);

  // Classic Way
{
  Runner.Clear;
  Runner.MigrationList.Add(TUsersMgr_202301010001.Create);
  Runner.MigrationList.Add(TUsersMgr_202301010002.Create);
  Runner.MigrationList.Add(TUsersMgr_202301010003.Create);

  Runner.MigrationList.Add(TCustomersMgr_202301010005.Create);
  Runner.MigrationList.Add(TCustomersMgr_202301010010.Create);

  Runner.MigrationList.Add(TInvoicesMgr_202301010005.Create);
  Runner.MigrationList.Add(TInvoicesMgr_202301010010.Create);
}
```

- Run the Migrator
```delphi
  Runner.UpgradeDatabase; // Do upgrade
  //OR
  Runner.DowngradeDatabase(202301010001); // Do downgrade to a specific version.
  //This version and lower versions of the database will remain and any version above this will be restored.
```
  
</details>  

<details>
  <summary>
   🔵 PostgreSQL Sample
  </summary>
<br>
  
- Initializing
```delphi
var
  LvConnectionParams: TPgConnectionParams;
begin
  with LvConnectionParams do // Could be loaded from ini, registry or somewhere else.
  begin
    Server := '192.168.212.1';
    LoginTimeout := 30000;
    Port := 5432;
    UserName := 'postgres';
    Pass := 'Admin123!@#';
    DatabaseName := 'Library';
    Schema := 'public';
  end;

  Runner := TPgRunner.Create(LvConnectionParams);
  Runner.Config
    .LogAllExecutions(True) // Optional
    .UseInternalThread(True) //Optional
    .SetProgressbar(pbTotal); //Optional

  {Use this line if you don't need local log}
  Runner.AddLogger.OnLog := OnLog;

  {Use this line if you need local log}
  //Runner.AddLogger.ConfigLocal(True, 'C:\Temp\EasyDBLog.txt').OnLog := OnLog;
end;
```
- Define migrations in diffrent place(unit)
```delphi
uses
  System.SysUtils,

  EasyDB.Core,
  EasyDB.MigrationX,
  EasyDB.Attribute,
  UHelper;

type

  [TCustomMigrationAttribute('TbUsers', 202301010001, 'Created users table', 'Alex')]
  TUsersMgr_202301010001 = class(TMigrationX)
  public
    procedure Upgrade; override;
    procedure Downgrade; override;
  end;

  [TCustomMigrationAttribute('TbUsers', 202301010002, 'Added newfielad1', 'Alex')]
  TUsersMgr_202301010002 = class(TMigrationX)
  public
    procedure Upgrade; override;
    procedure Downgrade; override;
  end;

  [TCustomMigrationAttribute('TbUsers', 202301010003, 'Added newfielad2', 'Alex')]
  TUsersMgr_202301010003 = class(TMigrationX)
  public
    procedure Upgrade; override;
    procedure Downgrade; override;
  end;

implementation

{ TUsersMgr_202301010001 }
procedure TUsersMgr_202301010001.Downgrade;
begin
  try
    PG.ExecuteAdHocQuery('DROP TABLE IF EXISTS public.tbusers;');
  except on E: Exception do
    Logger.Log(atDowngrade, E.Message, AttribEntityName, AttribVersion);
  end;
end;

procedure TUsersMgr_202301010001.Upgrade;
var
  LvScript: string;
begin
  LvScript := 'CREATE TABLE IF NOT EXISTS public.tbusers' + #10
              + '(' + #10
              + 'id SERIAL PRIMARY KEY,' + #10
              + 'username VARCHAR(100),' + #10
              + 'pass VARCHAR(50)' + #10
              + ');';

  try
    PG.ExecuteAdHocQuery(LvScript);
  except on E: Exception do
    Logger.Log(atUpgrade, E.Message, AttribEntityName, AttribVersion);
  end;
end;

{ TUsersMgr_202301010002 }
procedure TUsersMgr_202301010002.Downgrade;
begin
  try
    PG.ExecuteAdHocQuery('ALTER TABLE public.tbusers DROP COLUMN IF EXISTS CreatedDate;');
  except on E: Exception do
    Logger.Log(atUpgrade, E.Message, AttribEntityName, AttribVersion);
  end;
end;

procedure TUsersMgr_202301010002.Upgrade;
begin
  try
    PG.ExecuteAdHocQuery('ALTER TABLE public.tbusers ADD COLUMN CreatedDate TIMESTAMP;');
  except on E: Exception do
    Logger.Log(atUpgrade, E.Message, AttribEntityName, AttribVersion);
  end;
end;

{ TUsersMgr_202301010003 }

procedure TUsersMgr_202301010003.Downgrade;
begin
  try
    PG.ExecuteAdHocQuery('ALTER TABLE public.tbusers DROP COLUMN IF EXISTS ImageLink;');
  except on E: Exception do
    Logger.Log(atUpgrade, E.Message, AttribEntityName, AttribVersion);
  end;
end;

procedure TUsersMgr_202301010003.Upgrade;
begin
  try
    PG.ExecuteAdHocQuery('ALTER TABLE public.tbusers ADD COLUMN ImageLink VARCHAR(500);');
  except on E: Exception do
    Logger.Log(atUpgrade, E.Message, AttribEntityName, AttribVersion);
  end;
end;
```
- Add migrations
```delphi
  //Modern way
  Runner.Clear
  .Add(TUsersMgr_202301010001.Create)
  .Add(TUsersMgr_202301010002.Create)
  .Add(TUsersMgr_202301010003.Create)
  .Add(TCustomersMgr_202301010005.Create)
  .Add(TCustomersMgr_202301010010.Create)
  .Add(TInvoicesMgr_202301010005.Create)
  .Add(TInvoicesMgr_202301010010.Create);

  // Classic Way
{
  Runner.Clear;
  Runner.MigrationList.Add(TUsersMgr_202301010001.Create);
  Runner.MigrationList.Add(TUsersMgr_202301010002.Create);
  Runner.MigrationList.Add(TUsersMgr_202301010003.Create);

  Runner.MigrationList.Add(TCustomersMgr_202301010005.Create);
  Runner.MigrationList.Add(TCustomersMgr_202301010010.Create);

  Runner.MigrationList.Add(TInvoicesMgr_202301010005.Create);
  Runner.MigrationList.Add(TInvoicesMgr_202301010010.Create);
}
```
- Run the Migrator
```delphi
  Runner.UpgradeDatabase; // Do upgrade
  //OR
  Runner.DowngradeDatabase(202301010001); // Do downgrade to a specific version.
  //This version and lower versions of the database will remain and any version above this will be restored.
```
 
</details>  

<details>
<summary>
   🟠 Oracle Sample
</summary>
<br>
  
- Initializing
```delphi
var
  LvConnectionParams: TOracleConnectionParams;
begin
  with LvConnectionParams do // Could be loaded from ini, registry or somewhere else.
  begin
    Server := '127.0.0.1';
    LoginTimeout := 30000;
    UserName := 'admin';
    Pass := '123';
    DatabaseName := 'Library';
  end;

  Runner := TOracleRunner.Create(LvConnectionParams);
  Runner.Config
    .LogAllExecutions(True)// Optional
    .UseInternalThread(True)// Optional
    .SetProgressbar(pbTotal);// Optional

  {Use this line if you don't need local log}
  Runner.AddLogger.OnLog := OnLog;

  {Use this line if you need local log}
  //Runner.AddLogger.ConfigLocal(True, 'C:\Temp\EasyDBLog.txt').OnLog := OnLog;
end;
```
- Define migrations in diffrent place(unit)
```delphi
uses
  System.SysUtils,

  EasyDB.Core,
  EasyDB.MigrationX,
  EasyDB.Attribute,
  UHelper;

type

  [TCustomMigrationAttribute('TbUsers', 202301010001, 'Created users table', 'Alex')]
  TUsersMgr_202301010001 = class(TMigrationX)
  public
    procedure Upgrade; override;
    procedure Downgrade; override;
  end;

  [TCustomMigrationAttribute('TbUsers', 202301010002, 'Added newfielad1', 'Alex')]
  TUsersMgr_202301010002 = class(TMigrationX)
  public
    procedure Upgrade; override;
    procedure Downgrade; override;
  end;

  [TCustomMigrationAttribute('TbUsers', 202301010003, 'Added newfielad2', 'Alex')]
  TUsersMgr_202301010003 = class(TMigrationX)
  public
    procedure Upgrade; override;
    procedure Downgrade; override;
  end;

implementation

{ TUsersMgr_202301010001 }
procedure TUsersMgr_202301010001.Downgrade;
begin
  try
    Oracle.ExecuteAdHocQuery('Drop Table TbUsers;');
  except on E: Exception do
    Logger.Log(atDowngrade, E.Message, AttribEntityName, AttribVersion);
  end;
end;

procedure TUsersMgr_202301010001.Upgrade;
var
  LvScript: string;
begin
  LvScript := 'CREATE TABLE tbusers' + #10
              + '(' + #10
              + 'id NUMBER(10) NOT NULL,' + #10
              + 'username VARCHAR2(100 CHAR),' + #10
              + 'pass VARCHAR2(50 CHAR),' + #10
              + 'CONSTRAINT tbusers_pkey PRIMARY KEY (id)' + #10
              + ');';

  try
    Oracle.ExecuteAdHocQuery(LvScript);
  except on E: Exception do
    Logger.Log(atUpgrade, E.Message, AttribEntityName, AttribVersion);
  end;
end;

{ TUsersMgr_202301010002 }
procedure TUsersMgr_202301010002.Downgrade;
begin
  try
    Oracle.ExecuteAdHocQuery('ALTER TABLE TbUsers DROP COLUMN CreatedDate;');
  except on E: Exception do
    Logger.Log(atUpgrade, E.Message, AttribEntityName, AttribVersion);
  end;
end;

procedure TUsersMgr_202301010002.Upgrade;
begin
  try
    Oracle.ExecuteAdHocQuery('ALTER TABLE TbUsers ADD CreatedDate DATE;');
  except on E: Exception do
    Logger.Log(atUpgrade, E.Message, AttribEntityName, AttribVersion);
  end;
end;

{ TUsersMgr_202301010003 }

procedure TUsersMgr_202301010003.Downgrade;
begin
  try
    Oracle.ExecuteAdHocQuery('ALTER TABLE TbUsers DROP COLUMN ImageLink;');
  except on E: Exception do
    Logger.Log(atUpgrade, E.Message, AttribEntityName, AttribVersion);
  end;
end;

procedure TUsersMgr_202301010003.Upgrade;
begin
  try
    Oracle.ExecuteAdHocQuery('ALTER TABLE TbUsers ADD ImageLink VARCHAR2(500 CHAR);');
  except on E: Exception do
    Logger.Log(atUpgrade, E.Message, AttribEntityName, AttribVersion);
  end;
end;
```
- Add migrations
```delphi
  //Modern way
  Runner.Clear
  .Add(TUsersMgr_202301010001.Create)
  .Add(TUsersMgr_202301010002.Create)
  .Add(TUsersMgr_202301010003.Create)
  .Add(TCustomersMgr_202301010005.Create)
  .Add(TCustomersMgr_202301010010.Create)
  .Add(TInvoicesMgr_202301010005.Create)
  .Add(TInvoicesMgr_202301010010.Create);

  // Classic Way
{
  Runner.Clear;
  Runner.MigrationList.Add(TUsersMgr_202301010001.Create);
  Runner.MigrationList.Add(TUsersMgr_202301010002.Create);
  Runner.MigrationList.Add(TUsersMgr_202301010003.Create);

  Runner.MigrationList.Add(TCustomersMgr_202301010005.Create);
  Runner.MigrationList.Add(TCustomersMgr_202301010010.Create);

  Runner.MigrationList.Add(TInvoicesMgr_202301010005.Create);
  Runner.MigrationList.Add(TInvoicesMgr_202301010010.Create);
}
```
- Run the Migrator
```delphi
  Runner.UpgradeDatabase; // Do upgrade
  //OR
  Runner.DowngradeDatabase(202301010001); // Do downgrade to a specific version.
  //This version and lower versions of the database will remain and any version above this will be restored.
```

</details>  


- # ORM
### This library includes a mini Object-Relational Mapping (ORM) tool that can assist you in modernizing and simplifying your database upgrades and downgrades.(<ins>SQL SERVER</ins>, <ins>MySQL</ins>, and <ins>MariaDB</ins> only).

### Please feel free to utilize any other ORM of your choice if you don't prefer the internal one, such as [mORMot](https://github.com/synopse/mORMot), [TMS Aurelius](https://www.tmssoftware.com/site/aurelius.asp), etc...

<details>
  <summary>
   🔵 SQL SERVER Sample
  </summary>
<br>
- Initializing + Database creation

```delphi
var
  LvConnectionParams: TSqlConnectionParams;
  LvRunner: TSQLRunner;
  ORM: TORM;
begin
  with LvConnectionParams do // Could be loaded from ini, registry or somewhere else.
  begin
    Server := '192.168.212.1';
    LoginTimeout := 30000;
    UserName := 'sa';
    Pass := '1';
    DatabaseName := 'Master';
    Schema := 'dbo';
  end;

  LvRunner := TSQLRunner.Create(LvConnectionParams);
  LvRunner.Config
    .LogAllExecutions(True)// Optional
    .UseInternalThread(False)// Better to run with a single thread(Only for Database creation)
    .SetProgressbar(pbTotal)// Optional
    .DelayedExecution(500);// Optional

  LvRunner.AddLogger.OnLog := OnLog;

  try
    LvRunner.Clear;
    ORM := TORM.GetInstance(ttSQLServer);
    LvRunner.ORM := ORM;

    LvRunner.Add(TMigration.Create('Library DB', 202301010000, 'GodAdmin!', 'Created the Database',
    procedure
    begin
      with ORM do
      begin
        Create.Database('Library')
        .MdfFileName('C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\DATA\Library.mdf')
        .MdfSize('8192KB')
        .MdfFileGrowth('65536KB')
        .MdfMaxSize('UNLIMITED')
        .LdfFileName('C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\DATA\Library.ldf')
        .LdfSize('8192KB')
        .LdfFileGrowth('65536KB')
        .LdfMaxSize('2048GB')
        .Collation('Latin1_General_CI_AS');

        SubmitChanges;
      end;
    end,
    procedure
    begin
      ORM.Delete.Database('Library');
      ORM.SubmitChanges;
    end
    ));

    LvRunner.UpgradeDatabase;
  finally
    LvRunner.Free;
  end;
end;
```

- Add migrations

```delphi
var
  ORM: TORM;
begin
  InitializeRunner;
  TLogger.Instance.Log(atUpgrade, '');

  Runner.Clear;
  ORM := TORM.GetInstance(ttSQLServer);
  Runner.ORM := ORM;

  Runner.Add(TMigration.Create('TbUsers', 202301010001, 'Ali', 'Created table Users(#2701)',
  procedure
  begin
    with ORM do
    begin
      Create.Table('TbUsers').WithIdColumn
      .WithColumn('UserName').AsNvarchar(100).Nullable
      .WithColumn('Pass').AsNvarchar(50).Nullable;

      SubmitChanges;
    end;
  end,
  procedure
  begin
    ORM.Delete.Table('TbUsers');
    ORM.SubmitChanges;
  end
  ));
  Exit;
  //============================================
  Runner.Add(TMigration.Create('TbUsers', 202301010002, 'Ali', 'Added NewField2 to table Tbusers(#2702)',
  procedure
  begin
    ORM.Alter.Table('TbUsers').AddColumn('NewField2').AsVarchar(50).Nullable;
    ORM.SubmitChanges;
  end,
  procedure
  begin
    ORM.Alter.Table('TbUsers').DropColumn('NewField2');
    ORM.SubmitChanges;
  end
  ));
  //============================================
  Runner.Add(TMigration.Create('TbUsers', 202301010003, 'Ali', 'Added NewField3 to table Tbusers(#2703)',
  procedure
  begin
    ORM.Alter.Table('TbUsers').AddColumn('NewField3').AsInt.Nullable;
    ORM.SubmitChanges;
  end,
  procedure
  begin
    ORM.Alter.Table('TbUsers').DropColumn('NewField3');
    ORM.SubmitChanges;
  end
  ));
  //============================================
  Runner.Add(TMigration.Create('TbCustomers', 202301010003, 'Alex', 'Created Table TbCustomers and table TbInvoices(#2703)',
  procedure
  begin
    with ORM do
    begin
      Create.Table('TbCustomers')
      .WithColumn('Name').AsNvarchar(100).Nullable
      .WithColumn('Family').AsNvarchar(50).Nullable;

      Create.Table('TbInvoices').WithIdColumn
      .WithColumn('InvoiceNumber').AsBigInt.Nullable
      .WithColumn('InvoiceDate').AsDateTime.Nullable
      .WithColumn('MarketCode').AsInt.Nullable
      .WithColumn('TotalAmount').AsMoney.Nullable;

      SubmitChanges;
    end;
  end,
  procedure
  begin
    ORM.Delete.Table('TbCustomers');
    ORM.Delete.Table('TbInvoices');
    ORM.SubmitChanges;
  end
  ));

  //============================================
  Runner.Add(TMigration.Create('SelectTopTenCustomers', 202301010004, 'Alexander', 'Added SP and function(Task number #2704)',
  procedure
  var LvBody: string;
  begin
    with ORM do
    begin
      LvBody := 'Select * from TbInvoices where TotalAmount > @TotalAmount and MarketCode = @MarketCode and InvoiceDate = @ReportData';

      Create.StoredProc('SelectTopTenCustomers')
      .AddParam('TotalAmount', TDataType.Create(ctMoney))
      .AddParam('ReportData', TDataType.Create(ctDateTime))
      .AddParam('MarketCode', TDataType.Create(ctInt))
      .AddBody(LvBody);


      LvBody := 'Declare @Result Money '+ #10 +
                'Select @Result = Sum(TotalAmount) From TbInvoices where InvoiceDate <= @ReportData' + #10 +
                'Return @Result';

      Create.StoredFunction('GetTotalSum')
      .AddParam('ReportData', TDataType.Create(ctDateTime))
      .ReturnType(TDataType.Create(ctMoney))
      .AddBody(LvBody);

      SubmitChanges;
    end;
  end,
  procedure
  begin
    ORM.Delete.StoredProc('SelectTopTenCustomers');
    ORM.Delete.StoredFunc('GetTotalSum');
    ORM.SubmitChanges;
  end
  ));
end;
```  

- Run the Migrator

```delphi
  Runner.UpgradeDatabase; // Do upgrade
  //OR
  Runner.DowngradeDatabase(202301010001); // Do downgrade to a specific version.
  //This version and lower versions of the database will remain and any version above this will be restored.
``` 

</details>

<details>
  <summary>
   🟠 MySQL Sample
  </summary>
<br>
- Initializing

```delphi
var
  LvConnectionParams: TMySqlConnectionParams;
begin
  with LvConnectionParams do // Could be loaded from ini, registry or somewhere else.
  begin
    Server := '127.0.0.1';
    LoginTimeout := 30000;
    Port := 3306;
    UserName := 'ali';
    Pass := 'Admin123!@#';
    Schema := 'Library';
  end;

  Runner := TMySQLRunner.Create(LvConnectionParams);
  Runner.Config
    .LogAllExecutions(True) // Optional
    .UseInternalThread(True) //Optional
    .SetProgressbar(pbTotal) //Optional
    .DelayedExecution(500);

  {Use this line if you don't need local log}
  Runner.AddLogger.OnLog := OnLog;

  {Use this line if you need local log}
  //Runner.AddLogger.ConfigLocal(True, 'C:\Temp\EasyDBLog.txt').OnLog := OnLog;
```

- Add migrations

```delphi
var
  ORM: TORM;
begin
  Runner.Clear;
  ORM := TORM.GetInstance(ttMySQL);
  Runner.ORM := ORM;

  Runner.Add(TMigration.Create('TbUsers', 202301010001, 'Ali', 'Created table Users(#2701)',
  procedure
  begin
    with ORM do
    begin
      Create.Table('TbUsers').WithIdColumn
        .WithColumn('UserName').AsNvarchar(100).Nullable
        .WithColumn('Pass').AsNvarchar(50).Nullable;

      SubmitChanges;
    end;
  end,
  procedure
  begin
    ORM.Delete.Table('TbUsers');
    ORM.SubmitChanges;
  end
  ));
  //============================================
  Runner.Add(TMigration.Create('TbUsers', 202301010002, 'Ali', 'Added NewField2 to table Tbusers(#2702)',
  procedure
  begin
    ORM.Alter.Table('TbUsers').AddColumn('NewField2').AsVarchar(50).Nullable;
    ORM.SubmitChanges;
  end,
  procedure
  begin
    ORM.Alter.Table('TbUsers').DropColumn('NewField2');
    ORM.SubmitChanges;
  end
  ));
  //============================================
  Runner.Add(TMigration.Create('TbUsers', 202301010003, 'Ali', 'Added NewField3 to table Tbusers(#2703)',
  procedure
  begin
    ORM.Alter.Table('TbUsers').AddColumn('NewField3').AsInt.Nullable;
    ORM.SubmitChanges;
  end,
  procedure
  begin
    ORM.Alter.Table('TbUsers').DropColumn('NewField3');
    ORM.SubmitChanges;
  end
  ));
  //============================================
  Runner.Add(TMigration.Create('TbCustomers', 202301010003, 'Alex', 'Created Table TbCustomers and table TbInvoices(#2703)',
  procedure
  begin
    with ORM do
    begin
      Create.Table('TbCustomers')
        .WithColumn('Name').AsNvarchar(100).Nullable
        .WithColumn('Family').AsNvarchar(50).Nullable;

      Create.Table('TbInvoices').WithIdColumn
        .WithColumn('InvoiceNumber').AsBigInt.Nullable
        .WithColumn('InvoiceDate').AsDateTime.Nullable
        .WithColumn('MarketCode').AsInt.Nullable
        .WithColumn('TotalAmount').AsMoney.Nullable;

      SubmitChanges;
    end;
  end,
  procedure
  begin
    ORM.Delete.Table('TbCustomers');
    ORM.Delete.Table('TbInvoices');
    ORM.SubmitChanges;
  end
  ));

  //============================================
  Runner.Add(TMigration.Create('SelectTopTenCustomers', 202301010004, 'Alexander', 'Added SP and function(Task number #2704)',
  procedure
  var LvBody: string;
  begin
    with ORM do
    begin
      LvBody := 'Select * from TbInvoices where TotalAmount > @TotalAmount and MarketCode = @MarketCode and InvoiceDate = @ReportData;';

      Create.StoredProc('SelectTopTenCustomers')
        .AddParam('TotalAmount', TDataType.Create(ctMoney))
        .AddParam('ReportData', TDataType.Create(ctDateTime))
        .AddParam('MarketCode', TDataType.Create(ctInt))
        .AddBody(LvBody);


      LvBody := 'Declare Result DECIMAL(10, 2); '+ #10 +
                'Select Sum(TotalAmount) Into Result From TbInvoices where InvoiceDate <= @ReportData;' + #10 +
                'Return Result;';

      Create.StoredFunction('GetTotalSum')
        .AddParam('ReportData', TDataType.Create(ctDateTime))
        .ReturnType(TDataType.Create(ctDecimal, 10, 2), True)
        .AddBody(LvBody);

      SubmitChanges;
    end;
  end,
  procedure
  begin
    ORM.Delete.StoredProc('SelectTopTenCustomers');
    ORM.Delete.StoredFunc('GetTotalSum');
    ORM.SubmitChanges;
  end
  ));
end;
```
- Run the Migrator

```delphi
  Runner.UpgradeDatabase; // Do upgrade
  //OR
  Runner.DowngradeDatabase(202301010001); // Do downgrade to a specific version.
  //This version and lower versions of the database will remain and any version above this will be restored.
``` 

</details>

<details>
  <summary>
   🟣 MariaDb Sample
  </summary>
  <br>  
  
- Initializing

```delphi
var
  LvConnectionParams: TMariaDBConnectionParams;
begin
  with LvConnectionParams do // Could be loaded from ini, registry or somewhere else.
  begin
    Server := '127.0.0.1';
    LoginTimeout := 30000;
    Port := 3306;
    UserName := 'ali';
    Pass := 'Admin123!@#';
    Schema := 'Library';
  end;

  Runner := TMariaDBRunner.Create(LvConnectionParams);
  Runner.Config
    .LogAllExecutions(True) // Optional
    .UseInternalThread(True) //Optional
    .SetProgressbar(pbTotal) //Optional
    .DelayedExecution(500);

  {Use this line if you don't need local log}
  Runner.AddLogger.OnLog := OnLog;

  {Use this line if you need local log}
  //Runner.AddLogger.ConfigLocal(True, 'C:\Temp\EasyDBLog.txt').OnLog := OnLog;
end;
```

- Add migrations

```delphi
var
  ORM: TORM;
begin
  Runner.Clear;
  ORM := TORM.GetInstance(ttMariaDB);
  Runner.ORM := ORM;

  Runner.Add(TMigration.Create('TbUsers', 202301010001, 'Ali', 'Created table Users(#2701)',
  procedure
  begin
    with ORM do
    begin
      Create.Table('TbUsers').WithIdColumn
        .WithColumn('UserName').AsNvarchar(100).Nullable
        .WithColumn('Pass').AsNvarchar(50).Nullable;

      SubmitChanges;
    end;
  end,
  procedure
  begin
    ORM.Delete.Table('TbUsers');
    ORM.SubmitChanges;
  end
  ));
  //============================================
  Runner.Add(TMigration.Create('TbUsers', 202301010002, 'Ali', 'Added NewField2 to table Tbusers(#2702)',
  procedure
  begin
    ORM.Alter.Table('TbUsers').AddColumn('NewField2').AsVarchar(50).Nullable;
    ORM.SubmitChanges;
  end,
  procedure
  begin
    ORM.Alter.Table('TbUsers').DropColumn('NewField2');
    ORM.SubmitChanges;
  end
  ));
  //============================================
  Runner.Add(TMigration.Create('TbUsers', 202301010003, 'Ali', 'Added NewField3 to table Tbusers(#2703)',
  procedure
  begin
    ORM.Alter.Table('TbUsers').AddColumn('NewField3').AsInt.Nullable;
    ORM.SubmitChanges;
  end,
  procedure
  begin
    ORM.Alter.Table('TbUsers').DropColumn('NewField3');
    ORM.SubmitChanges;
  end
  ));
  //============================================
  Runner.Add(TMigration.Create('TbCustomers', 202301010003, 'Alex', 'Created Table TbCustomers and table TbInvoices(#2703)',
  procedure
  begin
    with ORM do
    begin
      Create.Table('TbCustomers')
        .WithColumn('Name').AsNvarchar(100).Nullable
        .WithColumn('Family').AsNvarchar(50).Nullable;

      Create.Table('TbInvoices').WithIdColumn
        .WithColumn('InvoiceNumber').AsBigInt.Nullable
        .WithColumn('InvoiceDate').AsDateTime.Nullable
        .WithColumn('MarketCode').AsInt.Nullable
        .WithColumn('TotalAmount').AsMoney.Nullable;

      SubmitChanges;
    end;
  end,
  procedure
  begin
    ORM.Delete.Table('TbCustomers');
    ORM.Delete.Table('TbInvoices');
    ORM.SubmitChanges;
  end
  ));

  //============================================
  Runner.Add(TMigration.Create('SelectTopTenCustomers', 202301010004, 'Alexander', 'Added SP and function(Task number #2704)',
  procedure
  var LvBody: string;
  begin
    with ORM do
    begin
      LvBody := 'Select * from TbInvoices where TotalAmount > @TotalAmount and MarketCode = @MarketCode and InvoiceDate = @ReportData;';

      Create.StoredProc('SelectTopTenCustomers')
        .AddParam('TotalAmount', TDataType.Create(ctMoney))
        .AddParam('ReportData', TDataType.Create(ctDateTime))
        .AddParam('MarketCode', TDataType.Create(ctInt))
        .AddBody(LvBody);


      LvBody := 'Declare Result DECIMAL(10, 2); '+ #10 +
                'Select Sum(TotalAmount) Into Result From TbInvoices where InvoiceDate <= @ReportData;' + #10 +
                'Return Result;';

      Create.StoredFunction('GetTotalSum')
        .AddParam('ReportData', TDataType.Create(ctDateTime))
        .ReturnType(TDataType.Create(ctDecimal, 10, 2), True)
        .AddBody(LvBody);

      SubmitChanges;
    end;
  end,
  procedure
  begin
    ORM.Delete.StoredProc('SelectTopTenCustomers');
    ORM.Delete.StoredFunc('GetTotalSum');
    ORM.SubmitChanges;
  end
  ));
```

- Run the Migrator

```delphi
  Runner.UpgradeDatabase; // Do upgrade
  //OR
  Runner.DowngradeDatabase(202301010001); // Do downgrade to a specific version.
  //This version and lower versions of the database will remain and any version above this will be restored.
```
</details>  


- # Large Script execution - (<ins>SQL SERVER</ins>, <ins>MySQL</ins>, and <ins>MariaDB</ins> only).

<details>
  <summary>
   🚩Important Note!
  </summary>
 
### To execute large scripts using this library, it is mandatory to separate each statement with the SQL Server-specific keyword "GO".
### If you have an extensive script, there's no problem. You can keep it as it is and proceed with this library going forward.
### To execute the current script with any size, you can refer to the corresponding sample project.(EasyDB_LargeScript_SQLServer, ...).
</details>

<details>
  <summary>
   🔵 SQL SERVER Sample
  </summary>


## Managing and running your script is a simple task that you can easily accomplish.  
```delphi
var
  LvConnectionParams: TSqlConnectionParams;
begin
  with LvConnectionParams do // Could be loaded from ini, registry or somewhere else.
  begin
    Server := '192.168.212.1';
    LoginTimeout := 30000;
    UserName := 'sa';
    Pass := '1';
    DatabaseName := 'AdventureWorks2019';
    Schema := 'dbo';
  end;

  TLogger.Instance.OnLog := OnLog;
  Runner := TSQLRunner.Create(LvConnectionParams);
  Runner.Config.UseInternalThread(True).LogAllExecutions(chkLogExecutions.Checked);
  Runner.SQL.ExecuteScriptFile('..\..\Script\AdventureWorks2019_Minimal.sql');
end;
```
  

![image](https://github.com/AliDehbansiahkarbon/EasyDB/assets/5601608/2d5b9113-8a9f-46e6-8c5d-2073bacfeac5)
</details>


<details>
  <summary>
   🟠 MySQL Sample
  </summary>

## Managing and running your script is a simple task that you can easily accomplish.
```delphi
var
  LvConnectionParams: TMySqlConnectionParams;
begin
  with LvConnectionParams do // The information can be sourced from an ini file, registry or other location.
  begin
    Server := '127.0.0.1';
    LoginTimeout := 30000;
    Port := 3306;
    UserName := 'ali';
    Pass := 'Admin123!@#';
    Schema := 'Library';
  end;

  TLogger.Instance.OnLog := OnLog;
  Runner := TMySQLRunner.Create(LvConnectionParams);
  Runner.Config.UseInternalThread(True).LogAllExecutions(GetLogStatus);
  Runner.MySQL.ExecuteScriptFile('..\..\Script\DBUpdateScript.sql', ';');
end;
```
</details>  

<details>
  <summary>
   🟣 MariaDb Sample
  </summary>

## Managing and running your script is a simple task that you can easily accomplish.
```delphi
var
  LvConnectionParams: TMariaDBConnectionParams;
begin
  with LvConnectionParams do // The information can be sourced from an ini file, registry or other location.
  begin
    Server := '127.0.0.1';
    LoginTimeout := 30000;
    Port := 3306;
    UserName := 'ali';
    Pass := 'Admin123!@#';
    Schema := 'Library';
  end;

  TLogger.Instance.OnLog := OnLog;
  Runner := TMariaDBRunner.Create(LvConnectionParams);
  Runner.Config.UseInternalThread(True).LogAllExecutions(GetLogStatus);
  Runner.MariaDB.ExecuteScriptFile('..\..\Script\DBUpdateScript.sql', ';');
end;
```
  
</details>  


# Last but not least!
### OnLog() - a very useful callback event.

There is an internal simple logger inside the library that is able to write log data in a text file but it has a beneficial event that will fire with each logging activity.
Using his event you can use your desired logging method like [QuickLogger](https://github.com/exilon/QuickLogger) or anything else and target any destination 
like [Graylog](https://www.graylog.org), [Betterstack](https://betterstack.com), Telegram, Email, etc...


# New Integration instruction
## Didn't find your database?
**No problem!** You can easily integrate this library with your environment by following the steps below:
- Create a new unit in this path ..\EasyDB\Lib\ConnectionManagers including a class that is inherited 
  from TConnection in the unit EasyDB.ConnectionManager.Base.pas, see the EasyDB.ConnectionManager.SQL.pas unit as a template.
- Create a new unit in this path ..\EasyDB\Lib\Runners including a class that is inherited from
  TRunner in the unit EasyDB.Runner.pas, see the EasyDB.MSSQLRunner.pas unit as a template.
- The two above steps are enough to use in simple and advanced modes but in case you prefer to use ORM form you need to create 
  a new function in the TBuilder class to generate scripts based on your target database, this class can be found
  in the path ..\EasyDB\Lib\ORM and unit EasyDB.ORM.Builder.pas.

# Last word!
If you find this library helpful, please do not hesitate to give me a rating by clicking on the star button.<br>
Additionally, keep an eye on the repository to stay updated on the latest features, bug fixes, and updates, While I welcome any pull requests, discussions, or issues, I kindly ask that you first review the closed issues to avoid any duplicates. Thank you for your trust.

Good luck!

<hr>
<p align="center">
<img src="https://dtffvb2501i0o.cloudfront.net/images/logos/delphi-logo-128.webp" alt="Delphi">
</p>
<h5 align="center">
Made with :heart: on Delphi
</h5>
