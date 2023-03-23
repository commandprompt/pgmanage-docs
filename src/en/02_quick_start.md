# Quick Start

## Install Quide

Download PgManage distribution file for your platform from the [Command Prompt website](https://www.commandprompt.com/products/pgmanage/).

### Linux

PgManage for Linux is packaged in `.AppImage` format and does not require installation. Download the `.AppImage` format, make it executable, and run it. For example, in an Ubuntu distribution:

```
chmod +x pgmanage-1.0.0a.AppImage
/pgmanage-1.0.0a.AppImage
```

### Windows

Run the PgManage setup executable and follow the installation instructions.

### Mac

Download the `DMG` file and double click on it and drag the PgManage icon to the `Applications Folder` icon.

### Oracle Support

A note about extra dependencies for Oracle support.
Application File locations (pgmanage.db pgmanage.log on different platforms)

## Launching the App

Once the application loads, it will prompt a message to set up a master password. Fill up the provided fields and press `Set master password`. This password will be requested the next time you open the application but you may reset it with a “Reset Master Password” button. 

![Setting up master password](./images/master_pass.png)

Next, you will be redirected to the welcome page:

![Welcome page with labels for the primary menu, utilities menu, walkthroughs](./images/main_pg.png)

To get started, you may press the information icon on the button right corner to access step-by-step walkthroughs.

The utilities menu is located at the top right corner. From there, you may create connections, access display settings, and manage plugins.

![Picture of the utilities menu](./images/utilities.png)

On the primary menu, one can manage connections and the snippets, which will be discussed later in this documentation.



Setting up the Master password (+ a brief explanation of credentials encryption and master password reset)
Creating your first connection. (need to explain some tech details on SSH tunneling, how passwords are used, .pgpass stuff etc)
Accessing Existing connections