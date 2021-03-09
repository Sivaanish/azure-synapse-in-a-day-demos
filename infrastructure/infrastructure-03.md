## Exercise 3: Visualize with Power BI

Time required: 15 minutes

![The visualize portion of the diagram is highlighted.](media/diagram-visualize.png "Visualize")

Power BI reports can be created, edited, and viewed from within Synapse Studio. This level of integration allows you to easily get data into your Power BI workspace. It also minimizes your need to switch tabs and navigate between services in the browser.

### Task 1: Connect Power BI Desktop

1. Return to the `synapse-lab-infrastructure` resource group and select the Azure Synapse Analytics workspace within.

    ![The Synapse workspace is highlighted in the resource group.](media/resource-group-synapse-workspace.png "Resource group")

2. In the **Overview** blade, copy the **Dedicated SQL endpoint** value.

    ![The SQL endpoint is highlighted.](media/synapse-workspace-sql-endpoint.png "Synapse Workspace")

3. Open Power BI Desktop. Close the sign-in window with the **X** on the upper-right corner.

    ![The X button is highlighted.](media/pbi-home.png "Power BI Desktop")

4. Select **Get Data**, then select **Azure** in the left-hand menu of the dialog that appears. Select **Azure Synapse Analytics (SQL DW)**, then select **Connect**.

    ![The options are highlighted as described.](media/pbi-get-data.png "Get Data")

5. Set the **Server** value to your copied SQL endpoint, and enter **aiaddw** for the **Database** name. Select the **DirectQuery** data connectivity mode, then click **OK**.

    ![The connection settings are configured as described.](media/pbi-connection.png "SQL Server database")

6. Select **Microsoft account** on the left-hand menu. **Sign in**, then click **Connect**.

    ![The authentication form is displayed.](media/pbi-auth.png "Authentication")

7. Click the box to the left of **DelaySummary**, which will load the data in the preview to the right. Click **Load**.

    ![Load the data.](media/pbi-load.png "Load")

### Task 2: Create a report

1. Click **Map** under Visualizations, then drag and drop **OriginAirportCode** into the **Legend** field. Refer to the table below for setting the other fields.

    | Parameters | Settings | Remarks |
    | --- | --- | --- |
    | Legend | `OriginAirportCode` | |
    | Latitude | `LATITUDE` | Select **Don't summarize** |
    | Longitude | `LONGITUDE` | Select **Don't summarize** |
    | Size | `DepDelayCount` | |

    ![The Map visualization is configured as described.](media/pbi-map.png "Map visualization")

    > **Note**: By default, latitude and longitude are treated as aggregate values. Click the drop-down arrow and select **Don't summarize**.

    ![The don't summarize menu option is highlighted.](media/pbi-do-not-summarize.png "Don't summarize")

2. Click an empty location on the report area. Under Visualizations, click **Stacked Column Chart**, then use the table below to set the field values.

    | Parameters | Settings | Remarks |
    | --- | --- | --- |
    | Axis | `DayofMonth` | |
    | Values | `DepDelayCount` | |

    ![The chart is configured as described.](media/pbi-stacked-column-chart.png "Stacked Column Chart visualization")

3. Click an empty location on the report area. Under Visualizations, click **Treemap**, then use the table below to set the field values.

    | Parameters | Settings | Remarks |
    | --- | --- | --- |
    | Group | `OriginAirportCode` | |
    | Values | `DepDelayCount` | |

    ![The visualization is configured as described.](media/pbi-treemap.png "Treemap visualization")

4. Clicking on a visual narrows down the data for the other visuals. (It may take a few seconds to load the data.)

    ![The LAS value is highlighted on the treemap, and the data for the other visuals is filtered based on the selection.](media/pbi-filter.png "The LAS value is highlighted on the treemap")

### Task 3: Save Power BI Desktop report

1. Click the **Save** icon on the top-left of Power BI Desktop. Enter a file name, such as **FlightDelays**, then click **Save** to save the report file.

    ![The Save icon and file save dialog are shown.](media/pbi-save.png "Save")

### Task 4: Create a Power BI workspace (optional)

If you have a Power BI Pro license and have create permissions, perform the following to create a new Power BI workspace for this lab. If not, you may skip the rest of the tasks in this exercise.

1. Sign in to Power BI service (https://powerbi.microsoft.com/landing/signin/)

2. Select **Workspaces**, then select **Create a workspace**.

    ![The create a workspace button is highlighted.](media/pbi-create-workspace-link.png "Create a workspace")

3. Type **AIAD_SynapseWorkspace** + `<unique id>` (where `<unique id>` is the unique id provided for your hosted lab environment, your initials, or any other value that ensures a unique name) for the workspace name, then select **Save**.

    ![The workspace name and save button are highlighted.](media/pbi-create-workspace.png "Create a workspace")

### Task 5: Create a Power BI Linked Service (optional)

If you were able to create a Power BI workspace, you can continue with this task. Otherwise, skip ahead to the next exercise.

1. Select the **Manage** hub, **Linked services**, then select **+ New**.

    ![The new linked service option is highlighted.](media/new-linked-service.png "New linked service")

2. Select **Power BI**, then select **Continue**.

    ![Power BI is highlighted.](media/new-linked-service-pbi.png "New linked service")

3. Make sure the correct Tenant is selected, then select the **AIAD_SynapseWorkspace** option from the **Workspace name** list. Select **Create** to continue.

    ![The form is configured as described.](media/new-linked-service-pbi-form.png "New linked service (Power BI)")

    > If the new Power BI workspace is not displayed, the tenant to which the AD account belongs and the tenant where the Power BI workspace is created may be different.

### Task 6: Create a Power BI dataset (optional)

If you were able to create a Power BI workspace, you can continue with this task. Otherwise, skip ahead to the next exercise.

1. Select the **Develop** hub.

    ![The Develop hub is selected.](media/develop-hub.png "Develop hub")

2. Expand the **Power BI** group, then expand the **AIAD_SynapseWorkspace** Linked Service. Select **Power BI datasets**, then select **+ New Power BI dataset** in the blade to the right.

    ![The new Power BI dataset button is highlighted.](media/new-pbi-dataset.png "New Power BI dataset")

3. Select **Start**.

    ![The Start button is highlighted.](media/new-pbi-dataset-start.png "New Power BI dataset")

4. Select the **aiaddw** SQL pool that you created, then select **Continue**.

    ![The aiaddw SQL pool and Continue button are highlighted.](media/new-bi-dataset-source.png "Select a data source")

5. Select **Download**. Open the downloaded Power BI Desktop data source (`.pbids`) file.

    ![The download button is highlighted.](media/new-pbi-dataset-download.png "Download pbids file")

6. Power BI Desktop launches when you open the `.pbids` file. When the navigator pops up, click the check box next to **DelaySummary**, then click **Load**.

    ![The Power BI dataset is loaded.](media/pbi-ds-navigator.png "Navigator")

7. Click **DirectQuery**, then click **OK**.

    ![The DirectQuery radio and OK button are highlighted.](media/pbi-ds-connection-settings.png "Connection settings")

8. Click **File**, then **Publish**. Click **Publish to Power BI**.

    ![The Publish dialog is displayed.](media/pbi-ds-publish.png "Publish")

9. Click **Save**.

    ![The Save button is highlighted.](media/pbi-ds-save.png "Save")

10. Enter a file name, such as **FlightDelaysDS**, then click **Save**.

    ![The save as dialog is shown.](media/pbi-ds-save-file.png "Save As")

11. Sign in to your Power BI account.

    ![The Power BI sign in form is displayed.](media/pbi-ds-sign-in.png "Sign in")

12. Select the **AIAD_SynapseWorkspace** workspace, then click **Select**.

    ![Select the workspace, then click select.](media/pbi-ds-select-workspace.png "Publish to Power BI")

13. When Power BI Desktop finishes publishing, sign in to the Power BI service (<https://powerbi.microsoft.com/landing/signin/>). Select the **AIAD_SynapseWorkspace** workspace on the left, then select the **Datasets + dataflows** tab. Select **...(More options)** under the Actions column of the `FlightDelaysDS` dataset, then select **Settings**.

    ![The dataset actions more options list is displayed with the Settings menu item highlighted.](media/pbi-more-options.png "More options")

14. Select **Edit credentials**.

    ![Edit credentials is highlighted.](media/pbi-dataset-settings.png "Settings")

15. Select the **OAuth2** authentication method, then select **Sign in**.

    ![The dataset credentials are configured as described.](media/pbi-dataset-credentials.png "Configure FlightDelaysDS")

16. Return to Synapse Studio, then select **Continue**.

    ![Continue is highlighted.](media/new-pbi-dataset-download-continue.png "Download pbids file")

17. Select **Close and refresh**.

    ![The close and refresh button is highlighted.](media/new-pbi-dataset-close.png "Close and refresh")

18. Select **Power BI datasets**, then select the **New Power BI report** icon on the `FlightDelaysDS` dataset.

    ![The New Power BI report icon is highlighted.](media/pbi-datasets-new-report.png "Power BI datasets")

19. When the screen for the new report opens, modify the report as you wish.

    ![The new Power BI report is displayed.](media/pbi-new-report.png "New report")
