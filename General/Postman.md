# Postman 

## Setup a mock server

1. Create a mock server in postman
2. Define the requests the Mockserver needs to be able to accept.
3. Copy the link and use it to send your mock requests.

## Request a bearer token for a D365 F&O environment.

### Requirements

- Have an application registration with the correct API permissions
    - DynamicsERP | 
        - Ax.fullAccess
        - Connector.fullAccess
        - CustomServices.FullAccess
        - OData.fullAccess
    - Graph
        - User.Read
- Obtain the tenant ID in which the application registration is created.

### Steps

1. Configure the application registration in your Microsoft entra ID applications form.
2. Assign a user account to the application. 
    - The user account must be enabled and needs to have the correct permissions.
3. Open Postman
4. Create a new POST request with the tenantID in the URL
    - `https://login.microsoftonline.com/:tenant_id/oauth2/v2.0/token`
5. Pass the following parameters to the request
    - grant type | *client_credentials*
    - scope | *<F&O environment url>/.default*
    - client_id | *app registration client id*
    - client_secret | *app registration secret* 
6. Send the request

### Additional

- Using the following code you can update the environment variables to use the bearer token you just received
```
var json = JSON.parse(responseBody);

pm.test("Get Azure AD Token", function () {
  !json.error && responseBody !== '' && responseBody !== '{}' && json.access_token !== '';
});

pm.environment.set("bearerToken", json.access_token);```




