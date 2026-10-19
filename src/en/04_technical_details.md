# Technical Information

## Application File Location

Audax stores data in the following files:

**audaxdm.db**: an SQLite3 database to store database connections, query history, and other application data.

**audaxdm.log**: the application log file is used for debugging and bug reporting.

Depending on the operating system Audax is running on, these files can be found under the following paths:

- **Linux:** ~/.audaxdm/audaxdm-app
- **macOS:** ~/.audaxdm/audaxdm-app
- **Windows:** %USERPROFILE%\\\\.audaxdm

> ℹ️ When upgrading from PgManage, existing data (~/.pgmanage, pgmanage.db, pgmanage.log) is migrated automatically to the new location and file names on first launch.

> ℹ️ The Windows uninstaller does not remove application data by default. To remove it completely, delete the %USERPROFILE%\\\\.audaxdm directory manually.
