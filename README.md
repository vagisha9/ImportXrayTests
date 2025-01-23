# How to import a TestNG / JUnit test result XML file to XRay
## From your local machine

> 🚨 Rate Limiting Issue
> * Due to the rate limiting, you may get `HTTP Status 429 – Too Many Requests` response while importing test results to Xray using your API token.
> * DevOps will provide a pipeline for this purpose which is granted exemption from rate limiting in Jira
> * See [Rate limiting](https://developer.atlassian.com/cloud/jira/platform/rate-limiting/) for more details

1. Clone this project

2. Copy a TestNG / JUnit test result XML file to the project under `test-result\`

3. Provide Jira information in your `pom.xml` file
    * Jira Cloud: it requires `clientId` and `clientSecret` and set `cloud` to `true`
    * Jira Server/DC: it requires `jiraBaseUrl` and `jiraToken`

> Jira Cloud

 ```xml
<configuration>
	<!-- Jira Cloud -->
	<cloud>true</cloud>
	<clientId>YOUR_JIRA_CLIENT_ID</clientId>
	<clientSecret>YOUR_JIRA_CLIENT_SECRET</clientSecret>
```

> Jira Server/DC

```xml
<configuration>
	<!-- Jira Server/DC -->
	<jiraBaseUrl>http://your-jira-url</jiraBaseUrl>
	<jiraToken>YOUR_JIRA_API_TOKEN</jiraToken>
```

4. Provide Xray project information and test result XML file name in your `pom.xml` file
   * You can assign the project to a specific test plan by passing the testPlanKey parameter
   * If you provide an existing testExecKey, this will not create a new Test Execution issue but replace the existing one

```xml
<configuration>
	...
	<!-- Project Setting -->
	<!-- Project Key -->
	<projectKey>YOUR_PROJECT_KEY</projectKey>

	<!-- (Optional) Test Plan Key -->
	<testPlanKey>YOUR_TEST_PLAN_KEY-XX</testPlanKey>

	<!-- (Optional) Test Execution Key -->
	<testExecKey>YOUR_TEST_EXEC_KEY-XX</testExecKey>

	<!-- testng|junit|nunit|xunit|robot|xunit|cucumber|behave -->
	<reportFormat>testng</reportFormat>

	<!-- test result file path. You can import multiple files by providing the value like test-result/*.xml-->
	<reportFile>test-result/testng_result_for_xray.xml</reportFile>
</configuration>
```

5. Run the below command from the Command Prompt (or Eclipse Terminal) to import the TestNG result to XRay

```
import.sh
```

6. You can verify the import result from the response in the console
```
response: {"testExecIssue":{"id":"1126763","key":"CDTXTP-1274","self":"https://jira.****.ca/rest/api/2/issue/1126763"}}
```
## From Jenkins
* You just need to provide the below Jenkins properties:
```properties
# XRay for JIRA Integration
serverInstance: Your_Xray_Jira_Instance

projectKey: YOUR_PROJECT_KEY
testPlanKey: YOUR_TEST_PLAN_KEY
testExecKey: YOUR_TEST_EXECUTION_KEY
reportFormat: testng
reportFile: YOUR_TEST_EXECUTION_RESULT_FILE_PATH.xml
```

# How to get Jira API Token
* Only Jira admin can create API Token
* See [Global Settings: API Keys]([https://docs.getxray.app/display/XRAYCLOUD/Global+Settings%3A+API+Keys#GlobalSettings:APIKeys-RegenerateClientSecret](https://docs.getxray.app/display/XRAYCLOUD/Global+Settings%3A+API+Keys))
