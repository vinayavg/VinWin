https://www.blazemeter.com/blog/how-to-integrate-your-github-repository-to-your-jenkins-project

Configuring GitHub
 
Step 1: go to your GitHub repository and click on ‘Settings’.
Step 2: Click on Webhooks and then click on ‘Add webhook’.
Step 3: in the ‘Payload URL’ field, paste your Jenkins environment URL. At the end of this URL add /github-webhook/. In the ‘Content type’ select ‘application/json’ and leave the ‘Secret’ field empty.
Step 4: in the ‘Which events would you like to trigger this webhook?’ choose ‘Let me select individual events.’ Then, check ‘Pull Requests’ and ‘Pushes’. At the end of this option, make sure that ‘Active’ is checked and click on ‘Add webhook’.

We're done with the configuration on GitHub’s side! Now let's move on to Jenkins.

Configuring Jenkins
 
Step 5: In Jenkins, click on ‘New Item’ to create a new project.
Step 6: Give your project a name, them choose ‘Freestyle project’ and finally click on ‘OK’.
Step 7: Click on the ‘Source Code Management’ tab.
Step 8: Click on Git and paste your GitHub repository URL in the ‘Repository URL’ field.
Step 9: Click on the ‘Build Triggers’ tab and then on the ‘GitHub hook trigger for GITScm polling’. Or, choose the trigger of your choice.

That's it! Your GitHub repository is integrated with your Jenkins project. You can now use any of the files found in the GitHub repository and trigger the Jenkins job to run with every code commit.


https://stackoverflow.com/questions/14274293/show-current-state-of-jenkins-build-on-github-repo/26910986#26910986

1. Go to GitHub, log in, go to Settings, Personal access tokens, click on Generate new token.
2. Check repo:status (I'm not sure this is necessary, but I did it, and it worked for me).
3Generate the token, copy it.

Make sure the GitHub user you're going to use is a repository collaborator (for private repos) or is a member of a team with push and pull access (for organization repos) to the repositories you want to build.

Go to your Jenkins server, log in.

Manage Jenkins → Configure System
Under GitHub Web Hook select Let Jenkins auto-manage hook URLs, then specify your GitHub username and the OAuth token you got in step 3.

screenshot of Jenkins global settings

Verify that it works with the Test Credential button. Save the settings.

Find the Jenkins job and add Set build status on GitHub commit to the post-build steps





In the meanwhile the UI of Jenkins and GitHub has changed a bit and it took me a while to figure out how to configure Jenkins now correctly. The explanation here is based on Jenkins version 2.121.1.

I also assume that you have already configured your Jenkins Job be triggered by a webhook or by polling. Those are the steps that I have taken to get it working:

Configure Github: Create Personal Access Token with OAuth Scope repo:status
Configure Jenkins: Configure System and add the OAuth Secret as a GitHub Server - use Secret Text as an authentication method to put the OAuth Secret in there.
Configure your Jenkins Job: Add Set GitHub commit status as Post-build action. Set the Status Result to One of the default messages and statuses.
Check your result on GitHub: Check if you get the build status and build execution duration on your GitHub commit.
