# **Jira API Documantion**

## **Authentication and authorization**
**Before You Start:**  
We are using basic authentication for jira api.  
To send a request using basic authentication, you'll need the following:
* A Jira Server site; I will use `<your-site>` for this documantion
* The username and API Token of a user who has permission to create issues on your Jira Server site

**Getting API Token:**  
https://id.atlassian.com/manage-profile/security/api-tokens

**Send a request from terminal:**  
to send a request to a Jira site as follows:

>curl -u mail:`<api-token>` -X GET -H "Content-Type: application/json" http://`<your-site>`/rest/api/2/issue/createmeta

if you get data's your api key and authentication is right after that you should use postman to get encoded authorization key and snippet code

## **POSTMAN**
**Getting encoded auth key:**  
Create a get request and at authorization pick basic auth after that;
> Username: `<jira-email>`  
Password: `<api-token>`  
Request URL: http://`<your-site>`/rest/api/2/issue/createmeta

