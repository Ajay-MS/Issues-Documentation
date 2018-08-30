## Endpoint Issues

### Issue #1
* Unable to delete an endpoint in VSTS account.

#### Workaround
The trick is to change the creation mode of the service endpoint from "Automatic" to "Manual". This can be done with the REST API in the following steps with examples from your VSTS account.

###### Get All Endpoints
GET - `https://<ACCOUNT_NAME>.visualstudio.com/<PROJECT GUID or PROJECT NAME>/_apis/distributedtask/serviceendpoints?api-version=3.2-preview.1`

Note down endpoint Id of endpoint you want to delete.

###### Get specific endpoint
GET - `https://<ACCOUNT_NAME>.visualstudio.com/<PROJECT GUID or PROJECT NAME>/_apis/distributedtask/serviceendpoints/<SERVICE_ENDPOINT_ID>?api-version=3.2-preview.1`

Copy output of endpoint and paste it in body for next request. Change value of "creationMode": "Manual". In my case I also had to remove the azureSpnRoleAssignmentId, spnObjectId and appObjectId fields completly from the data object.

###### Update endpoint type
PUT - `https://<ACCOUNT_NAME>.visualstudio.com/<PROJECT GUID or PROJECT NAME>/_apis/distributedtask/serviceendpoints/<SERVICE_ENDPOINT_ID>?api-version=3.2-preview.1`

Now from VSTS UI itself you can delete endpoint.

Reference - [Cannot remove service endpoints](https://developercommunity.visualstudio.com/content/problem/102182/cannot-remove-service-endpoints-for-subscription-t.html)
