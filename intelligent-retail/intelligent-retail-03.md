## Exercise 4: Data analysis

Time required: 15 minutes

### Synapse serverless SQL pool overview

It is possible to run queries directly on-demand for data in any format in the data lake. As shown in the following table, there are differences between serverless (on-demand) SQL pools and dedicated SQL pools:

| Feature | Synapse dedicated SQL pools | Synapse serverless SQL pools |
| --- | --- | --- |
| Connections | Yes | Yes |
| Resource classes and concurrency | Yes | No |
| Transactions | Yes | No |
| User-defined schemas | Yes | Yes |
| Table distribution | Yes | No |
| Table indexes | Yes |No |
| Table partitions | Yes | No |
| Statistics | Yes | Yes |
| CTAS | Yes | No |
| External tables | Yes | Yes |
| CETAS | Yes | Yes |

As an example, dedicated SQL pools are used for boilerplate analysis in day-to-day work, whereas serverless SQL pools are used for on-the-fly analysis.

![The two types of SQL pools are displayed.](media/sql-pools-diagram.png "SQL pools diagram")

### Task 1: Data exploration

1. Navigate to the **Data hub**.

    ![Data hub.](media/data-hub.png "Data hub")

2. Select the **Linked** tab **(1)**, expand the Azure Data Lake Storage Gen2 group, then expand the primary storage account for the workspace. Select the **sampledata** container **(2)**, then navigate to the **`tran/face/parquet`** folder **(3)**. Right-click on the **`face_data.snappy.parquet`** file **(4)**, select **New SQL script (5)**, then select **Select TOP 100 rows**.

    ![The file is highlighted.](media/linked-select-parquet.png "Select Parquet file")

3. **Run** the script and verify that you can view the file results.

    ![The file results are displayed.](media/serverless-sql-parquet-results.png "Parquet file script results")

4. **Copy the data lake account name** and save it to Notepad or similar text editor. You will use it in additional scripts below.

    ![The account name is highlighted.](media/data-lake-account-name.png "The account name is highlighted")

    When you run a SQL script on a serverless SQL pool, you can load the file data from the data lake and other sources and handle the data as if stored in a database.

    ![Data is loaded from the data lake with serverless SQL pools.](media/data-lake-serverless-sql.png "Load data from a data lake")

    > **Explore multiple files**
    >
    > By using a wildcard (*) for the file path specified by OPENROWSET, you can query multiple files that meet the criteria.
    >
    > Example: `https: //<storage name>.dfs.core.windows.net/<file system name>/tran/join/2020/*/*`

### Task 2: Data exploration in a variety of formats (CSV format)

Serverless SQL pools let you run queries for various data file formats, including CSV.

1. Replace the script with the following to query a CSV file. **Replace** `YOUR_ADLS_ACCOUNT_NAME` with the name of your workspace's primary data lake account that you copied in the previous step.

    ```sql
    SELECT *
    FROM OPENROWSET
    (
        BULK 'abfss://sampledata@YOUR_ADLS_ACCOUNT_NAME.dfs.core.windows.net/master/m_item/m_item.csv'
        , FORMAT = 'CSV'
        , FIELDTERMINATOR =','
        , ROWTERMINATOR = '\n'
        , FIRSTROW=2
    )
    WITH
    (
        shelf_id VARCHAR(100),
        sensor_no VARCHAR(100),
        item_genre VARCHAR(100),
        item_name VARCHAR(100),
        item_price INT
    ) AS [r]
    ```

2. **Run** the script and verify that you can view the file results.

    ![The query results are displayed.](media/serverless-sql-csv-results.png "CSV file results")

### Task 3: Data exploration in a variety of formats (JSON format)

1. Replace the script with the following to query a JSON file. **Replace** `YOUR_ADLS_ACCOUNT_NAME` with the name of your workspace's primary data lake account that you copied in the previous step.

    ```sql
    WITH SENSOR AS(SELECT 
        JSON_VALUE(jsonContent, '$.face_id') AS face_id,
        JSON_VALUE(jsonContent, '$.shelf_id') AS shelf_id,
        JSON_VALUE(jsonContent, '$.sensor_no') AS sensor_no,
        JSON_VALUE(jsonContent, '$.item_genre') AS item_genre,
        JSON_VALUE(jsonContent, '$.item_name') AS item_name,
        JSON_VALUE(jsonContent, '$.date_time') AS date_time,
        JSON_VALUE(jsonContent, '$.sensor_weight') AS sensor_weight,
        JSON_VALUE(jsonContent, '$.diff_weight') AS diff_weight
    FROM 
        OPENROWSET(
            BULK 'abfss://sampledata@YOUR_ADLS_ACCOUNT_NAME.dfs.core.windows.net/tran/sensor/*/*/*/*.json',
            FORMAT='CSV',
            FIELDTERMINATOR ='0x0b',
            FIELDQUOTE = '0x0b'
        )
        WITH (
            jsonContent varchar(300)
        ) AS [r]
    )
    SELECT TOP 10 * FROM SENSOR;
    ```

2. **Run** the script and verify that you can view the file results.

    ![The query results are displayed.](media/serverless-sql-json-results.png "JSON file results")

### Task 4: Data analysis

1. Replace the script with the following to analyze products popular with thirty-year-olds. **Replace** `YOUR_ADLS_ACCOUNT_NAME` with the name of your workspace's primary data lake account that you copied in the previous step.

    ```sql
    WITH SENSOR as (SELECT 
        JSON_VALUE(jsonContent, '$.face_id') AS face_id,
        JSON_VALUE(jsonContent, '$.shelf_id') AS shelf_id,
        JSON_VALUE(jsonContent, '$.sensor_no') AS sensor_no,
        JSON_VALUE(jsonContent, '$.item_genre') AS item_genre,
        JSON_VALUE(jsonContent, '$.item_name') AS item_name,
        JSON_VALUE(jsonContent, '$.date_time') AS date_time,
        JSON_VALUE(jsonContent, '$.sensor_weight') AS sensor_weight,
        JSON_VALUE(jsonContent, '$.diff_weight') AS diff_weight
    FROM 
        OPENROWSET(
            BULK 'abfss://sampledata@YOUR_ADLS_ACCOUNT_NAME.dfs.core.windows.net/tran/sensor/*/*/*/*.json',
            FORMAT='CSV',
            FIELDTERMINATOR ='0x0b',
            FIELDQUOTE = '0x0b'
        )
        WITH (
            jsonContent varchar(300)
        ) AS [r]
    ),
    FACE as (SELECT 
        JSON_VALUE(jsonContent, '$.face_id') AS face_id,
        JSON_VALUE(jsonContent, '$.shelf_id') AS shelf_id,
        JSON_VALUE(jsonContent, '$.age') AS age,
        JSON_VALUE(jsonContent, '$.gender') AS gender
    FROM 
        OPENROWSET(
            BULK 'https://YOUR_ADLS_ACCOUNT_NAME.dfs.core.windows.net/sampledata/tran/face/2020/04/01/',
            FORMAT='CSV',
            FIELDTERMINATOR ='0x0b',
            FIELDQUOTE = '0x0b'
        )
        WITH (
            jsonContent varchar(200)
        ) AS [r]
    )
    SELECT 
        top 5 f.age,
        s.item_name,
        count(*) as count
    FROM 
        SENSOR s
    JOIN FACE f ON s.face_id = f.face_id
    WHERE
        f.age = 30
    GROUP BY
        f.age, s.item_name
    ORDER BY
        f.age,count DESC
    ```

2. **Run** the script then select the **Chart** view in the query results.

    ![The query results are displayed.](media/serverless-sql-analysis-results.png "Analysis results")

3. In the Chart visualization form, set the **Chart type** to `Bar`, **Category column** to `item_name`, then **Legend (series) columns** to `count`.

    ![The chart visualization settings are highlighted.](media/serverless-sql-analysis-chart-view.png "Chart visualization")
