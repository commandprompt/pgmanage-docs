# Quick Start

## Install Quide

Download PgManage distribution file for your platform from the [Command Prompt website](https://www.commandprompt.com/products/pgmanage/).

### Linux

PgManage for Linux is packaged in `.AppImage` format and does not require installation.  
Download PgManage .AppImage file, in the command line change to the directory containing the downloaded file, make it executable, and run it:

```
chmod +x ./pgmanage-$version.AppImage
./pgmanage-$version.AppImage
```

### Windows

Run the PgManage setup executable and follow the installation instructions.  
FIXME: add postgresql command line tools installation instructions

### Mac

Download the `DMG` file and double click on it and drag the PgManage icon to the `Applications Folder` icon.  
FIXME: add postgresql command line tools installation instructions

### Oracle Support

A note about extra dependencies for Oracle support.


## Launching the App

When the application starts for the first time, it will prompt a message to set up a master password. Fill up the provided fields and press `Set master password`.  

This password will be requested the next time you open the application but you may reset it with a “Reset Master Password” button.  


![Setting up master password](./images/master_pass.png)

Next, you will be greeted with the application welcome page:

![Welcome page with labels for the primary menu, utilities menu, walkthroughs](./images/main_pg.png)

To get started, you may press the information icon on the button right corner to access the step-by-step walkthroughs.

The utilities menu is located at the top right corner. From there, you may access the application settings, view appliation version and general application info.

![Picture of the utilities menu](./images/utilities.png)

On the primary menu, one can manage connections, switch between active database sessions and access the snippets, which will be discussed later in this documentation.


## Protected Credentials Storage

PgManage stores the sensitive data such as database access credentials and SSH keys encrypted with a Master Password.
Resetting master password will erase all the protected application data.

## Creating your first DB connection
Click on the ⚡ icon on the left sidebar, the connection management UI will be shown:  
![Connection Management](./images/connection_mgr.png)  

Connections and Connection Groups are shown on the left, clicking on the left panel items shows the item's view/edit form.  

Click the `➕ Add` button, select Connection  
Set connection title and database type, the rest of the form will change depending on the database type selected.
Fill in the rest of database connection properties.  
**Note:** Alternatively the connection string may be used to establish a database connection.

There are two special connection types which behave differently:
 - SQLite connection does not need any other settings besides the sqlite3 file path.
 - Terminal connection is not a Database database connection, **requires** SSH tunnel properties to be set.

**Note:** The password field is optional, if you leave it empty the password prompt will be shown each time when establishing the connection. For Postgres connections, PgManage will also try to retrieve the connection password from *.pgpass* file.

### SSH Tunnelling
In addition to direct database connection PgManage also can connect to the database server via SSH tunnel. This feature can be useful in cases when the database server is not directly accessible, but can be accessed via intermediate SSH server host.
To use SSH tunnelling toggle the connesponding switch and enter the SSH credentials for the intermediate host.

### Testing and saving the Connection
The connection properties can be validated prior to connection save. To do so click the `Test` button on the top of the connections dialog.
If test passes, click `Save`.

### Connection Croups
Multiple related connections can be grouped.
To create a new group click  Click  `➕ Add` button and select the Group option.
On the Group form enter a name for the new connection group and select the connections to be grouped; click `Save`.
You may also group/ungroup a particular connection from the connection edit screen by selecting the corresponding option in the Group dropdown.

## Connecting to the database
You can access existing connections from in two ways:
 - From the connections menu when clicking `⚡` item on the left sidebar
 - From the connection management dialog, by clicking the connection item on the left and then clicking `Connect` button.