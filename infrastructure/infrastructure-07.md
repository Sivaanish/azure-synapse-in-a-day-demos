## Exercise 8: Visualize with Power BI

Time required: 10 minutes

![The SensorPredictions report is highlighted on the diagram.](media/diagram-visualize-sql-pool.png "Visualize")

Create a sensor summary report in Power BI.

### Task 1: Create connections and relationships

1. Return to the `synapse-lab-infrastructure` resource group and select the Azure Synapse Analytics workspace within.

    ![The Synapse workspace is highlighted in the resource group.](media/resource-group-synapse-workspace.png "Resource group")

2. In the **Overview** blade, copy the **SQL endpoint** value.

    ![The SQL endpoint is highlighted.](media/synapse-workspace-sql-endpoint.png "Synapse Workspace")

3. Open Power BI Desktop and open the **SensorOnDemand.pbix** report if it is not already open. Click **Get Data**, choose **Azure** from the left-hand menu, select **Azure Synapse Analytics (SQL DW)**, then click **Connect**.

    ![The Get Data dialog is displayed.](media/pbi-get-data-sensor.png "Get Data")

4. Set the **Server** value to your copied SQL endpoint, and enter **aiaddw** for the **Database** name. Select the **DirectQuery** data connectivity mode, then click **OK**.

    ![The connection settings are configured as described.](media/pbi-connection.png "SQL Server database")

5. **Optional**: If you are prompted to sign in, select **Microsoft account** on the left-hand menu. **Sign in**, then click **Connect**.

    ![The authentication form is displayed.](media/pbi-auth.png "Authentication")

6. Check the check boxes next to **PREDICT_SensorRUL** and **Sensor**, then click **Load**.

    ![The two tables are checked and the Load button is highlighted.](media/pbi-load-2-tables.png "Navigator")

7. If you get prompted by Power BI Desktop that adding new data sources presents a potential security risk, click **OK** to build the model.

    ![The OK button is highlighted.](media/pbi-security-risk.png "Potential security risk")

8. Select the **Modeling** tab, then click **Manage relationships** in the menu.

    ![Manage relationships is highlighted.](media/pbi-modeling-tab.png "Modeling tab")

9. Click **New**.

    ![The New button is highlighted.](media/pbi-relationships-new.png "Manage relationships")

10. Select the **PREDICT_SensorRUL** table in the first drop-down list, then select **Sensor** in the second drop-down list. Click **OK** to apply the table relationship. Notice that it automatically selected `DeviceId` as the relationship key and set the cardinality to `1:*`.

    ![The two tables are added and the OK button is highlighted.](media/pbi-relationships-predict-sensor.png "Create relationship")

11. Click **New** to create a new relationship.

    ![The New button is highlighted.](media/pbi-relationship-new2.png "Manage relationships")

12. Select the **PREDICT_SensorRUL** table in the first drop-down list, then select **v_sensor_ondemand** in the second drop-down list. Click **OK to apply the table relationship. This adds a relationship to the SQL serverless (On-demand) view you created earlier.

    ![The two tables are added and the OK button is highlighted.](media/pbi-relationships-predict-od.png "Create relationship")

13. Click **Close**.

    ![The Close button is highlighted.](media/pbi-relationship-new3.png "Manage relationships")

14. On the Report tab, click **+** to add a new report.

    ![The + button is highlighted on the report tab.](media/pbi-new-tab.png "New report")

15. Click an empty location in the report area. Under Visualizations, select **Table**, then use the table below to set the field values.

    | Parameters | Settings | Remarks |
    | --- | --- | --- |
    | Field | `DeviceId` | `PREDICT_SensorRUL` table |
    | Field | `RUL` | `PREDICT_SensorRUL` table |
    | Field | `avgSensor9` | `PREDICT_SensorRUL` table |
    | Field | `avgSensor11` | `PREDICT_SensorRUL` table |
    | Field | `avgSensor14` | `PREDICT_SensorRUL` table |
    | Field | `avgSensor15` | `PREDICT_SensorRUL` table |

    ![The chart is configured as described.](media/pbi-table.png "Table")

16. Click an empty location in the report area. Under Visualizations, select **Clustered column chart**, then use the table below to set the field values.

    | Parameters | Settings | Remarks |
    | --- | --- | --- |
    | Axis | `DeviceId` | `PREDICT_SensorRUL` table |
    | Legend | | |
    | Values | `Cycle` | `Sensor` table |

    ![The chart is configured as described.](media/pbi-clustered-column.png "Clustered column chart")

17. Click **Cycle** and select **Maximum**. This lets us verify how many cycles were sustained before required maintenance for each Device.

    ![The Maximum value is selected for Cycle.](media/pbi-cycle-max.png "Max of Cycle")

18. Click an empty location in the report area. Under Visualizations, select **Line chart**, then use the table below to set the field values.

    | Parameters | Settings | Remarks |
    | --- | --- | --- |
    | Axis | `Cycle` | `Sensor` table |
    | Legend | `DeviceId` | `PREDICT_SensorRUL` table|
    | Values | `Sensor11` | `Sensor` table |

    ![The chart is configured as described.](media/pbi-line-chart2.png "Line chart")

19. Click **Sensor11** to and select **Average** to make sure that the sensor blur is growing in the second half of the cycle prior to maintenance.

    ![The Average value is selected for Sensor11.](media/pbi-sensor11-avg.png "Average of Sensor11")

20. **Save** the report.
