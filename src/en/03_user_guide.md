# User Guide

![Welcome page with labels for the primary menu, utilities menu, walkthroughs](./images/main_pg.png)

---

## DB Entity Tree and Context Menus

The DB entity tree displays all of the database objects inside a server and is located to the right of primary menu once a database session is established.

![Tree context menu](./images/tree_context_menus.png)

The **context menus** can be accessed by right clicking on one of the tree nodes. The context menus allow you to make actions to the database. 

![Tree context menu when right clicking on an object](./images/context_menu.png)

---

## Tabs for Different Operations

At the right of the DB entity tree, the operation tabs are used to edit and execute the actions selected on the entity tree. A new tab can be opened by clicking on the plus sign on the tabs panel. There are four kinds of tabs:

### Query  
SQL queries can be written and executed to the database. Below the query editor, there are four buttons: `Run`, `Indent SQL`, `Command History`, `Explain`, and the `Explain Analyze`. The Run button executes the query and displays the query results below the query editor. The Indent SQL feature rewrites the query with standard indentation. The command history opens a menu where one can view and search executed queries. The Explain/Explain Analyze buttons provide information about the execution and planning time of the query, as well as information about the cost and filtering of rows. The information in the explain features will be displayed in a graph or in tabular form.  
**Note:** the Explain functionality is available for Postgres only.

![Query tab with the explain tree under it](./images/query_tab.png)

### Console  
Though the application allows to work with the database objects through the entity tree, a console may also be used to write commands directly.

![Console tab with a text editor under it](./images/console.png)

### Monitoring dashboard  
The monitoring tabs display various database performance mentrics graphs:

- Transaction Rate
- Backends
- Autovacuum Workers Usage
- WAL Production Rate
- Temp Files Creation Rate
- Autovacuum Freeze
- Blocked Locks
- Database Size
- Database Growth Rate
- Heap Cache Miss Ratio
- Index Cache Miss Ratio
- Seq Scan Ratio
- Long Transaction
- Long Query
- Long Autovacuum
- Checkpoints
- Bloat: Top 20 Tables
- In Recovery
- Autovac Freeze: Top 20 Tables
- Database Size
- Backends
- Activity

The number of graphs displayed can be controlled by clicking on `Manage units` and selecting the desired graphs. Alternatively, one can remove graph widgets from the dashboard by clicking the `❌` icon on the top-right of the widget. Personalized monitor units may be created by `Manage units → new unit`. This will open a new tab where the name, type, refresh interval, and template can be specified. Once a template has been selected, you may edit the script on the text editor beneath it.

![Display of the monitor unit creation form](./images/monitor_unit.png)
**Note:** the monitoring is available for Postgres, MariaDB and MySQL only.  

### Backends
The backends tab displays Postgres database sessions information such as back-end process id, start time, query and transaction start time, connected user etc. One can terminate a running back-end process by clicking the `❌` icon.  
**Note:** the Backends tab is available for Postgres only.

---

## Snippets

The snippets panel can be accessed by clicking on the snippet symbol on the right side menu. You may write code on the text editor and save it. The indentation feature will automatically rewrite the SQL in a pattern.

![Image displaying the snippets panel](./images/snippet.png)

To use a saved snippet, open a query tab and right click on the query editor. Here you may use, overwrite, or create a snippet.

![Image showing how to use snippets on the query tab](./images/snippet1.png)

---

## Command History

The command history is available in the query and console tabs. It displays a list of previously executed queries/commands in a given time range. To copy a query from the history, right click and select `Copy Content To Query Tab`.

![Image showing the query history](./images/history.png)

---

## Explain

The **Explain** and **Explain Analyze** features provide a visualization of the PostgreSQL execution plan for a query. The Explain feature provides the visualization with cost information while the Explain Analyze feature provides extra information -- such as time, number of rows filtered, and buffers. 

You may access the graph metrics by clicking on `Settings` at the top right. This allows to display information about cost, time, and rows. Next, you may click on a graph node to display more information about it. Finally, zoom in and out of the graph with the scroll wheel.

![Screenshot for the explain analyze feature](./images/explain_analyze.png)

---

## Code Autocompletion

To aid at typing speed, the application will suggest the names of available database object and SQL keywords while you type. Once the autocomplete menu is shown the suggested option can be selected by the up/down arrow keys and then inserted by pressing `Tab`

![Image showing the code autocompletion](./images/autocomplete.png)

---

## PostgreSQL Configuration Management
FIXME: replace screens with light-themed equivalents  

PgManage provides the convenient user interface to Postgres' ALTER SYSTEM set of commands via the **Server Configuration* context menu item.  
To access the server configuration for a PostgreSQL connection, right click on the server object on the DB entity tree and select `Server Configuration`.

![Image with pointer clicking the Server Configuration button](./images/configuration.png)

A new tab will open with the server configuration settings. You may search for a particular setting, filter available settings by category, apply config changes or revert to a previously saved configuration snapshot via the `Config History` drop down menu.  
After making the desired changes to the configuration click `Apply` to save the configuration.
A confirmation prompt with the list of config changes to be made will be shown, her you can provide a name for the current configuration snapshot. Once committed, the server configuration changes will be applied and the snapshot of previous configuration should appear in the `Config History` drop down menu.  
One may decide to revert some of previous configuration snapshots. To do so please select the snapshot from the dropdown menu click revert button and confirm the operation.  
**Note:** changes to some server configuration options may require Postgres server restart, in such cases a the list of restart-pending options will be shown.

![Screenshot of the Server Configuration interface](./images/configuration1.png)

---

## Data Editor Grid

The data editor grid allows editing the data of a specified query in a visual format. This feature can be accessed by right clicking on a table in the DB entity tree.

![Image illustrating how to access the data editor grid](./images/edit_data.png)

Then, a new tab will be created with a default query of `select * from mytable t`. A text editor allows to add further conditions to the query.

![Image showing the data grid tab](./images/edit_data1.png)

The data editor grid allows the following changes:
- Delete a row: click on the `x` icon and the selected rows will become red.
- Edit cells: double click on a data cell to edit the grid. Or, `right click → Edit Content` to open a separate editor.
- Displayed rows: click on the `Query Rows` drop down menu and select the number of desired rows.

Once the desired changes are done, click on the `Save` button.

---

## Exporting Query Data

Once a query has been entered, the resulting data can be exported into `.CSV` and `.XLSX` files. To export data, select the format at the button right of the query tab and click the download button.

![Close up to exporting data drop down menu](./images/exporting_data.png)

Finally, save the file under the data tab.

![Image demonstrating how to save the exported data as a CSV file](./images/exporting_data1.png)

---

## SQL Templates

The application automatically creates a template for actions selected in the DB Entity Tree. 
For example, a new record can be created by right clicking on a table object and selecting `Data Actions → Insert record`. For a given table, the following text was generated in a query tab:

![Image showing the autogenerated template for an INSERT query](./images/template1.png)

Once the desired information has been filled out, the query may be executed with the `run` button and a `INSERT 0 1` message will be displayed at the Data tab.

![Image showing the generated template with example data](./images/template2.png)

> **Note:** In future releases, this feature is planned to be re-implemented in an user interface.