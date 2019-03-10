# batchsqlpackage
This is an enhancement to a repository from John De Havilland's sqlpackageonazurebatch
Simple setup using templates to setup an Azure Batch Pool, install sqlpackage and then run tasks using it to restore databases.

1.  Create a Resource Group and add the following two services into it

First create a batch account in Azure

Second create a storage account

2. In the Batch account, under applications upload a zip of the 32bit DACFramework msi file calling it sqlpackage and setting the version to 1.0. You will also need to rename the msi to DACFramework-32bit.msi. Download the latest msi from [here](https://www.microsoft.com/en-us/download/details.aspx?id=55255)

3. Run the following from the command line: az batch pool create --template sql_pool_create_template.json

4. Enter a pool id and number of nodes.

5. When complete, run the following command: az batch job create --template sql_job_template.json --parameters job-params.json (make sure to update job-params with your parameters)
