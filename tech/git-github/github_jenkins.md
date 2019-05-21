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


