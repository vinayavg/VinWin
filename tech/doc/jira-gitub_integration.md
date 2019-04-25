

# Integrating Jira with Github 

https://confluence.atlassian.com/adminjiracloud/connect-jira-cloud-to-github-814188429.html


**Step 1**. Create an OAuth access token for your GitHub Enterprise account
The DVCS Connector requires an OAuth access token, which you create in your GitHub Enterprise account. You should create the access token in the GitHub Enterprise account that owns the repositories you want to link. If you are linking repositories for a team, you should generate this token using the team account. 

Create the OAuth token as follows:

Log in to GitHub Enterprise as a user with admin permissions on the account.
Choose Edit Your Profile. 
Select OAuth Applications.
Select the Developer Applications tab.
Choose Register new OAuth application. 
Enter a name for Application Name. 
Enter the Jira Software URL for both the URL and Callback URL fields. Use lower case. Press Register Application.

Make sure you enter the Jira Software Base URL (for example, https://myjiracloud.atlassian.net/) for both the Homepage URL and Authorization callback URL fields. Don't use the dashboard URL, such as https://myjiracloud.atlassian.net/secure/Dashboard.jspa. 

Keep your browser open in your GitHub Enterprise account while you go on with the next step.


**Step 2**. Add the OAuth token in Jira Software
After you create a key and secret in GitHub Enterprise, you go back to Jira Software and enter the account, the OAuth key, and secret as follows:

Log in to Jira Software as a user with admin permissions.
From the Jira Software dashboard click the cog (settings) icon.
Choose Applications.
From the Integrations section on the left, choose DVCS accounts.
Click Link GitHub Enterprise account.
Enter a your GitHub User Account name.

Enter your GitHub Enterprise site URL as the Host URL.
Copy the Client ID and Client Secret values from your GitHub Enterprise site into the dialog.

Leave the default auto link and Smart Commits (recommended) as is or change them:

Image showing the GitHub Enterprise link screen
 
Click Add. If you get redirected to a blank page at this point, see DVCS connection to GitHub produces blank page.

Grant access when prompted.
When Jira connects successfully, you'll see your account on the 'DVCS accounts' page.
The account you just connected and all of its repositories appears in the 'Managed DVCS Accounts' page. The initial synchronization starts automatically. After that, the system continues to sync your repository automatically on a regular basis.



(not for GHE at this point) : https://github.com/integrations/jira



### misc

try for 30 days free : https://www.atlassian.com/try/cloud/signup?bundle=jira-software

https://confluence.atlassian.com/adminjiracloud/configuring-workflow-triggers-776636696.html

https://marketplace.atlassian.com/apps/1219592/github-for-jira

https://www.atlassian.com/blog/jira-software/github-for-jira

https://github.blog/2018-10-04-announcing-the-new-github-and-jira-software-cloud-integration/

https://www.idalko.com/jira-github-integration/

https://confluence.atlassian.com/bitbucket/connect-bitbucket-cloud-to-jira-software-cloud-814190686.html
