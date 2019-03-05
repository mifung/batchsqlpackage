# batchsqlpackage
This is an enhancement to a repository from John De Havilland's sqlpackageonazurebatch
Simple setup using templates to setup an Azure Batch Pool, install sqlpackage and then run tasks using it to restore databases.
First create a batch account in Azure
In the Batch account, under applications upload a zip of the 32bit DACFramework msi file calling it sqlpackage and setting the version to 1.0. You will also need to rename the msi to DACFramework-32bit.msi. Download the latest msi from here
Run the following from the command line: az batch pool create --template sql_pool_create_template.json
Enter a pool id and number of nodes.
When complete, run the following command: az batch job create --template sql_job_template.json --parameters job-params.json (make sure to update job-params with your parameters)
