
--------------------------------------------------------------

Dir structure could look like

LR_Scenario

LR_Scripts

workspace/LoadRunnerTest/

downloaded & configured for instance .. slave.jar

-------------------------------------------------------------- 

Add build step

Execute HP test from file system/

Path could look like 

Tests: D:\jenkins\LR_Scenario\BlahBlar.lrs

--------------------------------------------------------------
 
Confiugure 

Post build action : Publish HP test result ; arch for failed test
