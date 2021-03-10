## Exercise 9: End processing

Time required: 5 minutes

You have reached the end of the lab. Follow the steps below to end processing and pause the SQL pool to save costs if you need to keep the lab environment available for additional labs.

> **Note**: If you no longer need the lab environment, delete the `synapse-lab-infrastructure` resource group to remove all lab resources.

### Task 1: Pause the SQL pool

1. Return to Synapse Studio (<https://web.azuresynapse.net/>) and select the **Manage** hub.

    ![The manage hub is selected.](media/manage-hub.png "Manage hub")

2. Select **SQL pools** then select the Pause button (**||**) on the `aiaddw` SQL pool.

    ![The pause button is highlighted.](media/pause-sql-pool.png "SQL pools")

### Task 2: Stop Stream Analytics

1. Navigate to the `synapse-lab-infrastructure` resource group in the Azure portal. Locate and open the **Stream Analytics job**.

    ![The Stream Analytics job is highlighted.](media/resource-group-stream-analytics.png "Stream Analytics job")

2. In the **Overview** blade, select **Stop** then **Yes** to confirm.

    ![The Stop button and Yes button are highlighted.](media/stream-analytics-stop.png "Stop job")

### Task 3: Stop Virtual Devices application

1. Stop the **IoTVirtualDevices** application by clicking **Virtual Device Processing** or closing the window to exit the app.

    ![The app window is shown.](media/virtual-device-processing.png "Virtual Devices")
