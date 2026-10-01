## How to Connect Power BI to SQLite (`.db` File)

Importing data into Power BI from a local SQLite `.db` file requires configuring an ODBC driver, as native SQLite
support is not built into Power BI Desktop.

---

### Step 1: Install the SQLite ODBC Driver

1. Go to the official driver download page: [ch-werner.de/sqliteodbc/](http://www.ch-werner.de/sqliteodbc/)
2. Download **`sqliteodbc_w64.exe`** (64-bit installer for Windows).
3. Run the installer and complete the setup wizard.

---

### Step 2: Configure the Windows ODBC Data Source

1. Press `Win + S` on Windows, search for **ODBC Data Sources (64-bit)**, and open it.
2. Under the **User DSN** tab, click **Add...**
3. Select **SQLite3 ODBC Driver** from the list and click **Finish**.

4. In the configuration window:

* **Data Source Name:** Enter a recognizable name (e.g., `ExecPulseDB`).
* **Database Name:** Click **Browse** and select your `.db` file (e.g., `data/processed/execpulse_star_schema.db`).

5. Click **OK** to save the DSN, then click **OK** to close the ODBC Administrator.

---

### Step 3: Load Data into Power BI Desktop

1. Open **Power BI Desktop**.
2. Go to **Home** $\rightarrow$ **Get data** $\rightarrow$ **More...** $\rightarrow$ **Other** $\rightarrow$ **ODBC**,
   then click **Connect**.
3. In the Data Source Name (DSN) dropdown, select `ExecPulseDB` and click **OK**.

4. On the credentials prompt screen:

* Select **Default or Custom** from the left-hand menu.
* Leave the fields blank (SQLite does not use user credentials).
* Click **Connect**.

5. In the **Navigator** window, check the tables you want to import (`Dim_Customer`, `Dim_Date`, `Dim_Product`,
   `Fact_Sales`).
6. Click **Load** to import all database tables into Power BI.

---
