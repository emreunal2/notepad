# **Jira API Documentation**

This documentation writed by Emre Unal for Londonist Global Management Portal.

## **Authentication and authorization**

**Before You Start:**  
We are using basic authentication for jira api.  
To send a request using basic authentication, you'll need the following:

* A Jira Server site; I will use `<your-site>` for this documantion
* The username and API Token of a user who has permission to create issues on your Jira Server site

**Getting API Token:**

[Getting API token](https://id.atlassian.com/manage-profile/security/api-tokens)  
(Datas will be avaible just for token-owners seen projects so admin should create token)  
**Send a request from terminal:**  
to send a request to a Jira site as follows:

>curl -u mail:`<api-token>` -X GET -H "Content-Type: application/json" http://`<your-site>`.atlassian.net/rest/api/2/issue/createmeta

if you get data's your api key and authentication is right after that you should use postman to get encoded authorization key and snippet code

## **POSTMAN**

**Getting encoded auth key:**  
Create a get request and at authorization pick basic auth after that;
> Username: `<jira-email>`  
Password: `<api-token>`  
Request URL: http://`<your-site>`.atlassian.net/rest/api/2/issue/createmeta

After that at the headers tab Authorization Key is your encoded api key.  
And at the code collum you can get your snippet code. Its something like that:

``` python
import requests

url = "https://your-site.atlassian.net/rest/api/2/issue/createmeta"

payload={}
headers = {
  'Authorization': 'Basic <encoded-api-key>',
  'Cookie': 'atlassian.xsrf.token=<encoded-cookie>'
}

response = requests.request("GET", url, headers=headers, data=payload)
```

## **Helper. py**

In `helper.py` we had lots of requests and functions i will explain all of requests, but for every function we should code setup as like this:

``` python
result = {}

    payload = "{}"
    headers = {
        'Content-Type': 'application/json',
        'Authorization': 'Basic <encoded-api-key>',
        'Cookie': 'atlassian.xsrf.token=<encoded-cookie>'
    }
```

## **How to set Query URLS**

To get urls use JQL (Jira Query Language)  
There is [Documantion](https://www.atlassian.com/blog/jira-software/jql-the-most-flexible-way-to-search-jira-14), and there is [JQL Cheat Sheet](https://3kllhk1ibq34qk6sp3bhtox1-wpengine.netdna-ssl.com/wp-content/uploads/2017/12/atlassian-jql-cheat-sheet-2.pdf)  
**Example:**  
So as an example I want to get **total backlog tasks of a project**, first thing first we should write our fix jql url

>https://<<your-site>>.atlassian.net/rest/api/2/search?jql=

After that we want to get `your-project and backlogged task`  
So `project=your-project-id AND status=BACKLOG`
When we encode this to url this become like this:  
`project%3Dyour-project-id%20AND%20status%3DBACKLOG`
So our url is:  
`https://<<your-site>>.atlassian.net/rest/api/2/search?jql=project%3Dyour-project-id%20AND%20status%3DBACKLOG`

## **jiraTotalTasks()**

Function that about total tasks from all jira.  
**Total Tasks:**  
Getting all tasks and takes total number as `'total_tasks'`

```python
url = "https://<<your-site>>.atlassian.net/rest/api/2/search?jql="
total_tasks = requests.request("GET", url, headers=headers, data=payload).json()
result['total_tasks'] = total_tasks['total']
```

**Total Backlog Tasks:**  
Getting All backloged tasks and takes total number as `'total_backlog_tasks'`

```python
url = "https://<<your-site>>.atlassian.net/rest/api/2/search?jql=(status%3DBACKLOG)"
total_backlog_tasks = requests.request("GET", url, headers=headers, data=payload).json()
result['total_backlog_tasks'] = total_backlog_tasks['total']
```

**Total In Progress Tasks:**  
Getting All In Progress tasks and takes total number as `'total_inprogress_tasks'`

```python
url = "https://<<your-site>>.atlassian.net/rest/api/2/search?jql=status%20%3D%20%22In%20Progress%22"
total_inprogress_tasks = requests.request("GET", url, headers=headers, data=payload).json()
result['total_inprogress_tasks'] = total_inprogress_tasks['total']
```

**Total Done Tasks:**  
Getting All Done tasks and takes total number as `'total_done_tasks'`

```python
url = "https://<<your-site>>.atlassian.net/rest/api/2/search?jql=(status%3DDONE)"
total_done_tasks = requests.request("GET", url, headers=headers, data=payload).json()
result['total_done_tasks'] = total_done_tasks['total']
```

## **jiraAllProjects()**

```python
url = "https://<<your-site>>.atlassian.net/rest/api/3/project/search?jql=&maxResults=200"
total_projects = requests.request("GET", url, headers=headers, data=payload).json()
result['total_projects'] = total_projects['total']
result['projects_names'] = total_projects['values']
```

With that code we get number of projects as `result['total_projects']` and get all project datas to `result['projects_datas']`,  
After that with a for loop in projects_datas we print all project names like this:

```python
for i in result['projects_datas']:
    print(i['name'])
```

## **jiraSingleProjects(projectId)**

This function doing almost same thing as **jiraTotalTask**, but doing it for just one project.
**Project Datas:**
Getting all datas of single project as `result['project']`

```python
project_url = "https://<<your-site>>.atlassian.net/rest/api/3/project/" + str(projectId)
project = requests.request("GET", project_url, headers=headers, data=payload).json()
result['project'] = project
```

**Project total tasks:**  
Getting Total tasks of single project as `result['total_tasks']`

```python
project_total_url = "https://<<your-site>>.atlassian.net/rest/api/2/search?jql=project=" + str(projectId)
project_total = requests.request("GET", project_total_url, headers=headers, data=payload).json()
result['total_tasks'] = project_total['total']
```

**Project Backlog Tasks:**  
Getting  backlog tasks of single project as `result['project_backlog']`

```python
project_backlog_url = "https://<<your-site>>.atlassian.net/rest/api/2/search?jql=project=" + str(
projectId) + "+and+status%3DBACKLOG"
project_backlog = requests.request("GET", project_backlog_url, headers=headers, data=payload).json()
result['project_backlog'] = project_backlog['total']
```

**Project In Progress Tasks:**  
Getting  backlog tasks of single project as `result['project_inprogress']`

```python
project_inprogress_url = "https://<<your-site>>.atlassian.net/rest/api/2/search?jql=project=" + str(
    projectId) + "+and+status%20%3D%20%22In%20Progress%22"
project_inprogress = requests.request("GET", project_inprogress_url, headers=headers, data=payload).json()
result['project_inprogress'] = project_inprogress['total']
```

**Project Done Tasks:**  
Getting  done tasks of single project as `result['project_done']`

```python
project_done_url = "https://<<your-site>>.atlassian.net/rest/api/2/search?jql=project=" + str(
    projectId) + "+and+status%3DDONE"
project_done = requests.request("GET", project_done_url, headers=headers, data=payload).json()
result['project_done'] = project_done['total']
```

## **jiraLatestIssues()**

Getting issues that created in last 24 hours

```python
url = "https://<<your-site>>.atlassian.net/rest/api/2/search?jql=created>%3D-1d"
jiraLatest = requests.request("GET", url, headers=headers, data=payload).json()
result = jiraLatest
```
