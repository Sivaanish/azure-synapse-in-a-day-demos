## Exercise 5: Data visualization

Time required: 15 minutes

### Task 1: Login to Power BI

1. In a new browser tab, navigate to <https://powerbi.microsoft.com/>.

2. Sign in with the same account used to sign in to Azure by selecting the **Sign in** link on the upper-right corner.

### Task 2: Create a Power BI workspace

1. Select **Workspaces**, then select **Create a workspace**.

    ![The create a workspace button is highlighted.](media/pbi-create-workspace.png "Create a workspace")

2. Set the name to **SynapseRetail** + `<unique id>` (where `<unique id>` is the unique id provided to you for your hosted environment, or your initials), then select **Save**.

    ![The form is displayed.](media/pbi-create-workspace-form.png "Create a workspace")

### Task 3: Connect to Power BI from Synapse

1. Switch back to Synapse Studio, then navigate to the **Manage hub**.

    ![Manage hub.](media/manage-hub.png "Manage hub")

2. Select **Linked services** on the left-hand menu, then select **+ New**.

    ![The new button is highlighted.](media/new-linked-service.png "New linked service")

3. Select **Power BI**, then select **Continue**.

    ![The Power BI service type is selected.](media/new-linked-service-power-bi.png "New linked service")

4. In the dataset properties form, complete the following:

    | Field                          | Value                                              |
    | ------------------------------ | ------------------------------------------         |
    | Name | _enter `handson_powerbi`_ |
    | Workspace name | _select `SynapseRetail` + `<unique id>` (the name of the workspace you created)_ |

    ![The form is displayed.](media/new-linked-service-power-bi-form.png "New linked service")

5. Select **Create** and then click on **Publish all**.

### Task 4: Create a Power BI dataset

1. Navigate to the **Develop hub**.

    ![Develop hub.](media/develop-hub.png "Develop hub")

2. Expand the `Power BI` group, then expand **handson_powerbi**. Select **Power BI datasets**, then select **+ New Power BI dataset**.

    ![The new Power BI dataset button is highlighted.](media/new-pbi-datasetu.png "New Power BI dataset")

3. If you see a dialog stating you need to install Power BI Desktop, select **Start** to continue.

    ![The Start button is highlighted.](media/pbi-dataset-start.png "Let's get started with Microsoft Power BI")

4. Under "Select a data source", select **SqlPool**, then select **Continue**.

    ![The SqlPool data source is selected.](media/pbi-dataset-datasource.png "Select a data source")

5. Select **Download** to save `SynapseRetailSqlPool.pbids` to a local folder.

    ![The Download button is highlighted.](media/pbi-dataset-download.png "Download .pbids file")

6. Open the downloaded **.pbids** file.

    ![The file is highlighted.](media/pbi-open-pbids.png "File Explorer")

7. After Power BI Desktop launches, you will be prompted to enter credentials for the dataset. Select **Microsoft account** in the left-hand menu, then click **Sign in** and enter your credentials you are using for this lab.

    ![The Microsoft account and Sign in elements are highlighted.](media/pbi-sign-in.png "Sign in")

8. After signing in, click **Connect**.

    ![The connect button is highlighted.](media/pbi-sign-in-next.png "Connect")

9. In the Navigator, check **t_item_count** and **t_shelf_count**, then click **Load**.

    ![The tables are checked.](media/pbi-navigator.png "Navigator")

10. Select **Import**, then click **OK**.

    ![The import option is selected.](media/pbi-import-ok.png "Connection settings")

11. After the data retrieval is complete, select **File**, then **Save as...**. Set the file name to **`sample1`**, then click **Save** to save the `.pbix` file locally.

    ![The file save dialog is displayed.](media/pbi-save-pbix.png "Save PBIX file")

12. Click **Publish**. If you are prompted to sign in, enter your email address and click **Sign in**. Use your same account you are using for this lab.

    ![The publish and sign in buttons are highlighted.](media/pbi-publish-signin.png "Publish - Sign in")

    > If the following error occurs, sign out and sign in again: "Only users with Power BI Pro licenses can publish to this workspace."

13. Select the **SynapseRetail** Power BI workspace, then click **Select**.

    ![The SynapseRetail workspace is selected.](media/pbi-publish-workspace.png "Publish to Power BI")

14. Return to Synapse Studio, then select **Refresh** and verify that the **`sample1`** dataset appears in the Power BI datasets list.

    ![The refresh button is highlighted.](media/pbi-datasets-list.png "Power BI datasets")

15. Expand **Power BI reports** in the left-hand menu, then select the **sample1** report.

    ![The selected report is displayed.](media/pbi-sample1.png "Power BI reports")

16. Create a visualization to show the ratio of men to women who stand in front of shelves. Select the **Pie chart** visualization. Expand `t_item_count`, then drag `gender` to **Legend** and `item_count` to **Values**.

    ![The pie chart settings are shown.](media/pbi-pie-chart.png "Pie chart")

17. With the pie chart selected, select the **Format** tab and configure the following:

    | Field                          | Value                                              |
    | ------------------------------ | ------------------------------------------         |
    | Title text | _enter `Gender Ratio`_ |
    | Font color | _select `White`_ |
    | Background color | _select `Navy blue`_ |
    | Alignment | _select `Center`_ |
    | Text size | _select `22 pt`_ |

    ![The pie chart is formatted as described.](media/pbi-pie-chart-format.png "Pie chart format")

### Task 5: Challenge - Add visualizations for age groups and genders

Referring to the steps in the previous task, add two new chart visualizations to your report that show the ratio of the number of people who stand in front of the shelf by age and gender.

1. Create two new visualizations with the following characteristics:

   - **Field:** `t_item_count` table:
     - `age`
     - `gender`
     - `item_count`
   - **Visualization:** Donut chart
   - **Title:**
     - `Ratio by Age Group (Male)`
     - `Ratio by Age Group (Female)`

2. Adjust the chart format and colors as preferred.

3. Use the **Filters** pane on each chart to filter on only males or females, respectively.

When you are done, your charts should look similar to the following:

![The two new charts are displayed as described.](media/pbi-chart-ratio-by-age-group.png "Ratio by age group charts")

### Task 6: Challenge - Add visualization for changes in the number of times product touched

Add a new visualization to show the changes in the number of times the product has been picked up per timeframe.

1. Create one new visualization with the following characteristics:

   - **Field:** `t_shelf_count` table:
     - `date`
     - `count`
     - `shelf_id`
   - **Visualization:** Line chart
   - **Title:** `Changes in Number Times Product Touched`

2. Adjust the chart format and colors as preferred.

When you are done, your chart should look similar to the following:

![The new chart is displayed as described.](media/pbi-chart-num-times-product-touched.png "Changes in Number Times Product Touched")

### Task 7: Challenge - Add visualization for visitor attribute analysis

Add a new table chart visualization to dhow the personal attributes (age and gender) of the number of times the product was touched.

1. Create one new visualization with the following characteristics:

   - **Field:** `t_item_count` table:
     - `item_genre`
     - `item_name`
     - `age`
     - `gender`
     - `item_count`
   - **Visualization:** Table
   - **Title:** `Visitor Attribute Analysis (by Product)`

2. Adjust the chart format and colors as preferred.

3. In "Values" on the Field tab, double-click the table item name to edit it. Edit the field names as follows:

    | Field                          | Rename to |
    | ------------------------------ | ------------------------------------------         |
    | `item_genre` | `Genre` |
    | `item_name` | `Name` |
    | `age` | `Age` |
    | `gender` | `Gender` |
    | `item_count` | `Times Touched` |

4. Click the name of the table item on the chart to change the sort key. Example: Select the number of times touched to sort in order of the number of times touched.

5. Select **Save** to save changes to your report.

    ![The save button is highlighted.](media/pbi-report-save.png "Save")

When you are done, your chart should look similar to the following:

![The new chart is displayed as described.](media/pbi-report-final.png "Visitor attribute analysis")
