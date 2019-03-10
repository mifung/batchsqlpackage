# batchsqlpackage
This is an enhancement to a repository from John De Havilland's sqlpackageonazurebatch
Simple setup using templates to setup an Azure Batch Pool, install sqlpackage and then run tasks using it to restore databases.

1.  Create a Resource Group <myresourcegroup> and add the following two services into it: <p>
    * First create a batch account in Azure 
    * Second create a storage account
    * Associate the storage account in the batch account by selecting the storage account you have just created

2. Download the latest DACFramework.msi from [here](https://www.microsoft.com/en-us/download/details.aspx?id=55255): <p>
    * Once downloaded, change the name to  DACFramework-32bit.msi to explicitly identify the downloaded version is 32 bit.  
    * Create a zip file from the DACFramework-32bit.msi to DACFramework-32bit.zip

3. Upload the following files: <p> 
    * In the Batch account under applications, upload the DACFramework-32bit.zip and call the application id 'sqlpackage' and then set the version to 1.0. 
    * Upload the SQL Server bacpac file to the storage account.

4.  In the Azure Portal, click on the Cloud shell button. <p>
    * Run the following command to add the batch cli extensions: `az extension add --name azure-batch-cli-extensions`
    * Authenticate Batch account to your CLI session: 
    `az batch account login \
    --resource-group myresourcegroup\
    --name batchaccountname\
    --shared-key-auth
    `

    * upload the following json files from cloud shell:
       1. sql_pool_create_template.json
       2. sql_job_template.json
       3. job-params.json

5. Run the following from the cloud shell command line: `az batch pool create --template sql_pool_create_template.json` <p>
   Enter:
       * pool id 
       * number of nodes

6. Type `code .` in the Cloud Shell command line to open visual studio code for editing the job-params.json files  
    * provide the pool-id that you have provided in step 4
    * provide a unique job-id
    * provide the data source, user id and password to the target SQL
    * provide the url to the bacpac file blob location
        1. go to the blob storage account where the bacpac file is located and click on the ellipsis and select Generate SAS
        2. change the date range to a longer duration if desired
        3. select 'Generate blob SAS token and URL'
        4. copy the blob SAS URL
        5. paste it to the bacpac value location between the double quotes in the job-params.json file

7. When completed, run the following command: `az batch job create --template sql_job_template.json --parameters job-params.json` 

