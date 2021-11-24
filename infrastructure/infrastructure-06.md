## Exercise 7: Spark ML learning/inference and SQL pool load

Time required: 10 minutes

![The ML and SQL Pool portions of the diagram are highlighted.](media/diagram-ml-sql-pool.png "ML and SQL Pool load")

In addition to ETL processing, you can run machine learning workloads using Spark ML within a Spark Notebook. You can also use the Azure Machine Learning Python SDK from within a Spark Notebook in Synapse to perform automatic machine learning, a feature of Azure Machine Learning.

### Task 1: Import and execute notebook

Train and run machine learning models in a Spark pool and load the predicted results into a SQL pool.

1. Select the **Develop** hub.

    ![The Develop hub is selected.](media/develop-hub.png "Develop hub")

2. Select **+**, then select **Import** from the drop-down menu.

    ![The Import menu item is selected.](media/develop-import.png "Import")

3. Navigate to the folder where you extracted the ZIP file for this lab. If you extracted the files to `C:\`, navigate to `C:\LabFiles\azure-synapse-in-a-day-demos-master\infrastructure\source\ETLandPREDICT` and select **TrainAndScoreRULModel.ipynb**.

    ![The TrainAndScoreRULModel file is highlighted.](media/trainmodel-file.png "Open")

4. Run the notebook to run SparkML and load the data into your SQL pool.
