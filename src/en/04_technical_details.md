# Technical Information

## Application File Location

PgManage stores data in the following files:

**pgmanage.db**: an SQLite3 database to store database connections, query history, and other application data.

**pgmanage.log**: the application log file is used for debugging and bug reporting.

Depending on the operating system PgManage is running on, these files can be found under the following paths:

- **Linux:** ~/.pgmanage/pgmanage-app
- **Mac OSX:** ~/.pgmanage/pgmanage-app
- **Windows:** %USERPROFILE%\\.pgmanage
