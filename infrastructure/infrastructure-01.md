## Exercise 2: Implement Spark ETL with the GUI

Time required: 45 minutes

![The mapping data flow is highlighted on the diagram.](media/diagram-etl-pipeline.png "ETL Pipeline")

Mapping data flows are visually designed data transformations in Azure Data Factory. Data flows allow data engineers to develop data transformation logic without writing code. The resulting data flows are executed as activities within Azure Data Factory pipelines that use scaled-out Apache Spark clusters. Data flow activities can be operationalized using existing Azure Data Factory scheduling, control, flow, and monitoring capabilities.

Mapping data flows provide an entirely visual experience with the Synapse Studio GUI, with no coding required. Your data flows run on ADF-managed execution clusters for scaled-out data processing. Azure Data Factory handles all the code translation, path optimization, and execution of your data flow jobs.

### Task 1: Create ADLS Gen2 Linked Service

1. Select the **Manage** hub, **Linked services**, then select **+ New**.

    ![The new linked service option is highlighted.](media/new-linked-service.png "New linked service")

2. Select **Azure Data Lake Storage Gen2**, then select **Continue**.

    ![Azure Blob Storage and the Continue button are highlighted.](media/new-linked-service-adls.png "New linked service")

3. Enter each setting described below, then select **Test connection**. When the connection is successful, select **Create**.

    ![The new linked service form is displayed.](media/new-linked-service-key-adls.png "New linked service")

    | Parameters | Settings | Remarks |
    | --- | --- | --- |
    | Name | `key_adls` | |
    | Description | No entry required | Default settings |
    | Connect via integration runtime | AutoResolveIntegrationRuntime | Default settings |
    | Authentication method | Account Key | Default settings |
    | Account selection method | From Azure subscription | Default settings |
    | Azure subscription | Any | Select the Azure subscription for this lab |
    | Storage account name | Any | Select the primary storage account for your Synapse workspace |

### Task 2: Create mapping data flow sources

Create datasets to extract "Flight delay" and "Airport master data".

1. Select the **Develop** hub.

    ![The Develop hub is selected.](media/develop-hub.png "Develop hub")

2. Select **+**, then select **Data flow**.

    ![The new Data flow is selected.](media/new-data-flow.png "New Data flow")

3. Select the **Data flow debug** switch.

    ![The data flow debug switch is highlighted.](media/data-flow-debug-switch.png "Data flow debug")

4. Select **OK** in the `Turn on data flow debug` dialog.

    ![The OK button is highlighted.](media/data-flow-debug-dialog.png "Turn on data flow debug")

5. Set **FlightDelayETL** for the Name in the Properties blade. Select **Properties** to close the blade, then select **Add Source** on the canvas.

    ![The name property is highlighted, along with the properties button and the Add Source box on the canvas.](media/flightdelay-name.png "Mapping data flow canvas")

6. For **Output stream name**, enter **AirportCodeLocationLookupClean**, and select **+ New** next to **Source dataset**.

    ![The source settings fields are highlighted.](media/flightdelay-airportcode-source.png "Source settings")

7. Select **Azure Data Lake Storage Gen2**, then select **Continue**.

    ![Azure Data lake Storage Gen2 and the Continue button are highlighted.](media/import-data-sink-new-adls.png "New dataset")

8. Select the **DelimitedText** format, then select **Continue**.

    ![Binary and the Continue button are highlighted.](media/import-data-source-new-adls-delimited.png "Select format")

9. Enter each setting as displayed in the table below, and then select **OK**.

    ![The form is completed as described in the table below.](media/flightdelay-new-adls-properties.png "Set properties")

    | Parameters | Settings | Remarks |
    | --- | --- | --- |
    | Name | `csv_AirportCodeLocationLookupClean` | Be careful not to include extra spaces when copying |
    | Linked service | Select the default storage for your workspace (`<Synapse workspace name>-WorkspaceDefaultStorage`) | |
    | File path | `datalake` | Select file from "From specified path" in the next step |

10. Select the **TravelDatasets** folder, then select **AirportCodeLocationLookupClean.csv** and select **OK**.

    ![The folder and file are selected as described.](media/flightdelay-new-adls-airportcodelookup.png "Choose a file or folder")

11. Check **First row as header**, ensure the **From connection / store** import schema option is selected, then select **OK**.

    ![The checkbox is highlighted.](media/flightdelay-airportcode-source-form.png "Set properties")

12. Select **Add Source** on the canvas below the source you just added.

    ![Add Source is highlighted on the canvas.](media/flightdelay-add-source.png "Add Source")

13. For **Output stream name**, enter **FlightDelaysWithAirportCodes**, and select **+ New** next to **Source dataset**.

    ![The output stream name and new button are highlighted.](media/flightdelay-withairportcodes-source.png "Source")

14. Select **Azure Data Lake Storage Gen2**, then select **Continue**.

    ![Azure Data lake Storage Gen2 and the Continue button are highlighted.](media/import-data-sink-new-adls.png "New dataset")

15. Select the **DelimitedText** format, then select **Continue**.

    ![Binary and the Continue button are highlighted.](media/import-data-source-new-adls-delimited.png "Select format")

16. Enter each setting as displayed in the table below, and then select **OK**.

    ![The form is completed as described in the table below.](media/flightdelay-withairportcodes-new-adls-properties.png "Set properties")

    | Parameters | Settings | Remarks |
    | --- | --- | --- |
    | Name | `csv_FlightDelaysWithAirportCodes` | Be careful not to include extra spaces when copying |
    | Linked service | Select the default storage for your workspace (`<Synapse workspace name>-WorkspaceDefaultStorage`) | |
    | File path | `datalake` | Select file from "From specified path" in the next step |

17. Select the **TravelDatasets** folder, then select **FlightDelaysWithAirportCodes.csv** and select **OK**.

    ![The folder and file are selected as described.](media/flightdelay-new-adls-withairportcodes.png "Choose a file or folder")

18. Check **First row as header**, ensure the **From connection / store** import schema option is selected, then select **OK**.

    ![The checkbox is highlighted.](media/flightdelay-withairportcodes-source-form.png "Set properties")

### Task 3: Create a filter

Create a filter for the records.

1. Under the `FlightDelaysWithAirportCodes` source, select **+** in the lower-right corner, and then select **Filter**.

    ![The plus button and Filter menu option are highlighted.](media/flightdelay-add-filter.png "Filter")

2. Under "Filter on", select **Enter filter...**.

    ![The enter filter box is highlighted.](media/flightdelay-filter-enter-filter.png "Filter settings")

3. Copy and paste the following query into the Expression box. Select **Refresh** to review the data preview, then select **Save and finish**.

    ```javascript
    toInteger(DepDelay) > 0
    ```

    ![The filter area, refresh button, and save button are all highlighted.](media/flightdelay-filter-builder.png "Visual expression builder")

### Task 4: Add calculated columns

In this data, a delay of `1:30` is represented as `130`. Create a delay time column with the name "CRSDepHour" so that it can be represented as a `1.3` hour delay.

1. Under the `FlightDelaysWithAirportCodes` source, select **+** in the lower-right corner of the stream (from the new filter you just created), and then select **Derived Column**.

    ![The plus button and Derived Column menu option are highlighted.](media/flightdelay-withairportcodes-add-derivedcolumn.png "Derived Column")

2. Under Columns, type **CRSDepHour**, select **Expression**, then select **Open expression builder**.

    ![The derived column's settings blade is displayed.](media/flightdelay-withairportcodes-derivedcolumn-columns.png "Derived column's settings")

3. Copy and paste the following query into the Expression box. Select **Refresh** to review the Data preview, and then select **Save and finish**.

    ```javascript
    floor(toInteger(CRSDepTime)/100)
    ```

    ![The CRSDEPHOUR expression builder is displayed.](media/flightdelay-withairportcodes-derivedcolumn-expression.png "Visual expression builder")

### Task 5: Select columns

Add an action to eliminate unnecessary columns so that only the required columns remain.

1. Under the `FlightDelaysWithAirportCodes` source, select **+** in the lower-right corner of the stream (from the new derived column action you just created), and then select **Select**.

    ![The plus button and Select menu option are highlighted.](media/flightdelay-withairportcodes-add-select.png "Select")

2. Check **DerivedColumn1's column** to check all columns. Then remove checks from the following nine items: Make sure that **all other items** besides the following nine are checked, and then select **Delete**.

    - Year
    - Month
    - DayofMonth
    - DayOfWeek
    - CRSDepTime
    - DepDelay
    - DepDel15
    - OriginAirportCode
    - CRSDepHour

![The items to delete are checked.](media/flightdelay-withairportcodes-select-columns.png "Select settings")

### Task 6: Aggregation process

Add aggregation processing so that delays are aggregated at the granularity of the year, month, day, and airport.

1. Under the `FlightDelaysWithAirportCodes` source, select **+** in the lower-right corner of the stream (from the new select action you just created), and then select **Aggregate**.

    ![The plus button and Aggregate menu option are highlighted.](media/flightdelay-withairportcodes-add-aggregate.png "Aggregate")

2. Select the drop-down arrow on the right of Columns to add the following five columns. Click **+** to add an item:

    - Year
    - Month
    - DayofMonth
    - DayOfWeek
    - OriginAirportCode

    ![The dropdown arrow and plus button are highlighted.](media/flightdelay-withairportcodes-aggregate-columns.png "Columns")

3. Verify that all five columns have been added, then select **Aggregates**.

    ![The Aggregates header is highlighted.](media/flightdelay-withairportcodes-aggregate-columns-aggregate.png "Aggregates")

4. In Columns, type **DepDelayCount**, select **Expression**, then paste `sum(1)`.

    ![The DepDelayCount column is highlighted.](media/flightdelay-withairportcodes-aggregate-add-depdelaycount.png "DepDelayCount")

5. Select **+** to the right of the column you just added, then select **Add column**.

    ![The add column menu item is highlighted.](media/flightdelay-withairportcodes-aggregate-add-column.png "Add column")

6. In Columns, type **DepDelay15Count**, select **Expression**, then paste `sum(toInteger(DepDel15))`.

    ![The DepDelayCount column is highlighted.](media/flightdelay-withairportcodes-aggregate-add-depdelay15count.png "DepDelayCount")

### Task 7: Data join process

Combine data sources.

1. Under the `FlightDelaysWithAirportCodes` source, select **+** in the lower-right corner of the stream (from the new aggregate action you just created), and then select **Join**.

    ![The plus button and Join menu option are highlighted.](media/flightdelay-add-join.png "Join")

2. Within the Join settings form, enter each setting as described in the table below.

    ![The Join settings form is configured as described in the table below.](media/flightdelay-join-settings.png "Join settings")

    | Parameters | Settings | Remarks |
    | --- | --- | --- |
    | Output stream name | `Join1` | Default settings |
    | Left stream | `Aggregate1` | |
    | Right stream | `AirportCodeLocationLookupClean` | This is the first data source you added |
    | Join type | `Inner` | Default settings |
    | Left: Aggregate1's column | `OriginAirportCode` | |
    | Right: AirportCodeLocationLookupClean's column | `AIRPORT` | |

### Task 8: Data output

Add a sink process to output to the SQL Pool and create a dataset that defines the destination table information.

1. Under the `FlightDelaysWithAirportCodes` source, select **+** in the lower-right corner of the stream (from the new join action you just created), and then select **Sink**.

    ![The plus button and Sink menu option are highlighted.](media/flightdelay-add-sink.png "Sink")

2. Select **+ New** next to the Sink dataset.

    ![The new dataset button is highlighted.](media/flight-delay-sink-dataset-new.png "Sink")

3. Select **Azure Synapse Analytics**, then select **Continue**.

    ![Azure Synapse Analytics is selected in the list.](media/flightdelay-sink-dataset-synapse.png "New dataset")

4. Enter each setting as displayed in the table below, and then select **OK**.

    ![The form is completed as described in the table below.](media/flightdelay-sink-dataset-properties.png "Set properties")

    | Parameters | Settings | Remarks |
    | --- | --- | --- |
    | Name | `dw_DelaySummary` | Be careful not to include extra spaces when copying |
    | Linked service | Select the default SQL server for your workspace (`<Synapse workspace name>-WorkspaceDefaultSqlServer`) | |
    | Table name | None | Default settings, set in subsequent steps |
    | Import schema | None | Default settings |

5. Select **Open** next to the Sink dataset.

    ![The Open button is highlighted.](media/flightdelay-sink-dataset-open.png "Sink")

6. Under the **Connection** tab of the dataset properties, enter **aiaddw** SQL pool name for the **DBName** value. Select **Refresh** next to the table list, then select **dbo.DelaySummary** from the list.

    ![The form values are configured as described.](media/dw-delaysummary-properties.png "dw_DelaySummary properties")

7. Select the **FlightDelayETL** tab to switch back to the mapping data flow. Select **Sink1**, select the **Settings** tab, then select **Truncate table** in the Table action options list. Make sure **Enable staging** is also checked.

    ![The form values are configured as described.](media/flightdelay-sink-settings.png "Sink settings")

8. Select **Publish all**, then select **Publish** in the dialog.

    ![Publish all is highlighted.](media/publish-all.png "Publish all")

### Task 9: Create and run Pipeline

1. Select the **Integrate** hub, select **+**, then select **Pipeline**.

    ![The new pipeline link is highlighted.](media/new-pipeline.png "New pipeline")

2. For **Name**, enter `FlightDelay`, then select **Properties** to close the dialog.

    ![The properties dialog is displayed.](media/flightdelay-pipeline-name.png "FlightDelay pipeline name")

3. Expand **Move & transform** under the Activities menu. Drag and drop the **Data flow** activity onto the canvas area to the right.

    ![The data flow activity has an arrow pointing to the canvas.](media/flightdelay-pipeline-add-data-flow.png "Add data flow activity")

4. In the Adding data flow dialog, select **Use existing data flow**, select **FlightDelayETL** from the existing data flow list, then select **Finish**.

    ![The form is configured as described.](media/flightdelay-pipeline-add-data-flow-dialog.png "Adding data flow")

5. Select the **Settings** tab, select **key_adls** from the **Staging linked service** list, then enter **datalake** in the container field and **polybase** in the folder field of the **Staging storage folder** setting.

    ![The settings form is configured as described.](media/flightdelay-pipeline-data-flow-settings.png "Data flow settings")

6. Select **Publish all**, then select **Publish** in the dialog.

    ![Publish all is highlighted.](media/publish-all.png "Publish all")

7. Select **Add trigger**, then select **Trigger now**.

    ![The trigger now option is highlighted.](media/flightdelay-pipeline-trigger-now.png "Trigger now")

8. Select **OK** to run the trigger.

    ![The OK button is highlighted.](media/import-data-trigger-pipeline-run.png "Pipeline run")

9. Select the **Monitor** hub, then **Pipeline runs**. If the pipeline run status is `Succeeded` after a few minutes, then the pipeline run successfully completed.

    ![The pipeline run successfully completed.](media/flightdelay-pipeline-monitor.png "Monitor pipeline runs")

> **Note**: Mapping Data Flow runs with a minimum overhead of at least five to six minutes. This is because you are creating a Spark processing environment each time you run it. If you are using an ETL pipeline using Mapping Data Flow, plan your batch schedule with time in mind for creating a processing environment.

> **Note**: Some of the imported data has a prepared dataset that has been amplified to 100 million items under the name "FlightDelaysWithAirportCodes100M.csv". With this csv data, you can see the scalability of Mapping Data Flow for 100 million data processes.

### Task 10: View the transformed data

When the data flow pipeline has successfully completed, view the data it wrote to the `dbo.DelaySummary` SQL pool table.

1. Select the **Data** hub.

    ![The data hub is selected.](media/data-hub.png "Data hub")

2. Select the **Workspace** tab, expand **SQL database**, expand the **aiaddw** SQL pool, and expand **Tables**. Right-click on the `DelaySummary` table, select  **New SQL script**, then select **Select TOP 100 rows**.

    ![The DelaySummary table is displayed.](media/delaysummary-table-new-scriptu.png "DelaySummary table")

3. View the results of the executed script. Notice that the derived columns, joined columns, and aggregates you added to the Mapping Data Flow appear in the result set.

    ![The query result set is displayed.](media/delaysummary-table-result-set.png "DelaySummary table result set")
