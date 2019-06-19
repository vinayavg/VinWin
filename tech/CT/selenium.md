https://tecadmin.net/

https://tecadmin.net/setup-selenium-chromedriver-on-ubuntu/

Setup Selenium with ChromeDriver

Step 1 – Prerequisites
Execute the following commands to install the required packages on your system. Here Xvfb (X virtual framebuffer) is an in-memory display server for a UNIX-like operating system (e.g., Linux). It implements the X11 display server protocol without any display. This is helpful for CLI applications like CI service.

sudo apt-get update
sudo apt-get install -y unzip xvfb libxi6 libgconf-2-4
Also, install Java on your system. Let’s install Oracle Java 8 on your system or use below command to install OpenJDK.

sudo apt-get install default-jdk 


Step 2 – Install Google Chrome
Now install Latest Google chrome package on your system using the below list commands. Google chrome headless feature opens multipe doors for the automation.

sudo curl -sS -o - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add
sudo echo "deb [arch=amd64]  http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list
sudo apt-get -y update
sudo apt-get -y install google-chrome-stable


Step 3 – Install ChromeDriver
You are also required to setup ChromeDriver on your system. ChromeDriver is a standalone server which implements WebDriver’s wire protocol for Chromium. The WebDriver is an open source tool for automated testing of web apps across multiple browsers.

wget https://chromedriver.storage.googleapis.com/2.41/chromedriver_linux64.zip
unzip chromedriver_linux64.zip
You can find the latest ChromeDriver on its official download page. Now execute below commands to configure ChromeDriver on your system.

sudo mv chromedriver /usr/bin/chromedriver
sudo chown root:root /usr/bin/chromedriver
sudo chmod +x /usr/bin/chromedriver



Step 4 – Download Required Jar Files
The Selenium Server is required to run Remote Selenium WebDrivers. You need to download the Selenium standalone server jar file using the below commands or visit here to find the latest version of Jar file.

wget https://selenium-release.storage.googleapis.com/3.13/selenium-server-standalone-3.13.0.jar
Also download the testng-6.8.7.jar file to your system.

wget http://www.java2s.com/Code/JarDownload/testng/testng-6.8.7.jar.zip
unzip testng-6.8.7.jar.zip


Step 5 – Start Chrome via Selenium Server
Your server setup is ready. Start the Chrome via standalone selenium server using Xvfb utility.

Run Chrome via Selenium Server

xvfb-run java -Dwebdriver.chrome.driver=/usr/bin/chromedriver -jar selenium-server-standalone.jar
Use -debug option at end of command to start server in debug mode.

You can also Start Headless ChromeDriver by typing the below command on terminal.

chromedriver --url-base=/wd/hub
Your Selenium server is now running with Chrome. Use this server to run your test cases written in Selenium using Google Chrome web browser. Next step is an optional step and doesn’t depend on Step 5.




Step 6 – Sample Java Program (Optional)
This is an optional step. It describes running a single test case using Selenium standalone server and ChromeDriver. Let’s create a Java program using Selenium server and Chrome Driver. This Java program will open a specified website URL and check if defined string presents on the webpage or not.

Create a Java program by editing a file in text editor.

vim TecAdminSeleniumTest.java
Add the below content to file.

import java.io.IOException;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.chrome.ChromeDriver;
import org.testng.annotations.Test;

public class TecAdminSeleniumTest {

        public static void main(String[] args) throws IOException, InterruptedException {
                System.setProperty("webdriver.chrome.driver", "/usr/bin/chromedriver");
                ChromeOptions chromeOptions = new ChromeOptions();
                chromeOptions.addArguments("--headless");
                chromeOptions.addArguments("--no-sandbox");

                WebDriver driver = new ChromeDriver(chromeOptions);

                driver.get("https://google.com");

                Thread.sleep(1000);

                if (driver.getPageSource().contains("I'm Feeling Lucky")) {
                        System.out.println("Pass");
                } else {
                        System.out.println("Fail");
                }
                driver.quit();
        }
}
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
import java.io.IOException;
 
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.chrome.ChromeDriver;
import org.testng.annotations.Test;
 
public class TecAdminSeleniumTest {
 
        public static void main(String[] args) throws IOException, InterruptedException {
                System.setProperty("webdriver.chrome.driver", "/usr/bin/chromedriver");
                ChromeOptions chromeOptions = new ChromeOptions();
                chromeOptions.addArguments("--headless");
                chromeOptions.addArguments("--no-sandbox");
 
                WebDriver driver = new ChromeDriver(chromeOptions);
 
                driver.get("https://google.com");
 
                Thread.sleep(1000);
 
                if (driver.getPageSource().contains("I'm Feeling Lucky")) {
                        System.out.println("Pass");
                } else {
                        System.out.println("Fail");
                }
                driver.quit();
        }
}
You can change the URL “https://google.com” with any other URL of your choice, Then also change the search string like “I’m Feeling Lucky” used in the above Java program. Save your java program and execute it. First, you need to set the Java CLASSPATH environment variable including the selenium-server-standalone.jar and testng-6.8.7.jar. Then compile the java program and run it.

export CLASSPATH=".:selenium-server-standalone.jar:testng-6.8.7.jar"
javac TecAdminSeleniumTest.java
java TecAdminSeleniumTest
You will see results like below. If the defined search string found, You will get message “Pass” and if string not found on the webpage, you will get the “Fail” message on the screen.









http://selftechy.com/2011/08/17/running-selenium-tests-with-chromedriver-on-linux



Running Selenium Tests with ChromeDriver on Linux

Some of the pre-requisites has to be setup to execute the Selenium WebDriver tests with chromedriver on Linux

Download the following Softwares before starting to write tests in eclipse.

Download Google Chrome – Chrome for Linux
Download ChromeDriver – ChromeDriver for Linux
Install the Google Chrome on the Linux ennvironment by using the following methods:

Double click or use rpm command if the package is “.rpm” (am currently using Fedora) to install the google chrome
Use apt-get / YUM command to download and then install the package for different Linux flavors accordingly
Executing ChromeDriver Server:

Inside /home/${user} – create a new directory “ChromeDriver”
Unzip the downloaded chromedriver into this folder
Using chmod +x filename or chmod 777 filename make the file executable
Go to the folder using cd command
Execute the chrome driver with ./chromedriver command
Now the chromedriver will start executing in the 9515 port
[seetaram@Linux chromedriver]$ ./chromedriver
Started ChromeDriver
port=9515
version=14.0.836.0
Above is the output of the chromedriver server executing in Linux terminal.

After the above is accomplished, try to setup the test on the eclipse

Download the Selenium server 2.0
Download JUnit
Unzip both the files and configure them to build path in the eclipse
Write the test code as below in the Eclipse – Java file
package com.selftechy.wdriver;

import java.io.File;
import java.io.IOException;
import java.net.*;

import org.junit.*;
import org.junit.runner.RunWith;
import org.junit.runners.BlockJUnit4ClassRunner;
import org.openqa.selenium.*;
import org.openqa.selenium.chrome.ChromeDriverService;
import org.openqa.selenium.remote.DesiredCapabilities;
import org.openqa.selenium.remote.RemoteWebDriver;

public class ChromeRemoteDriver {
    
    public static void main(String []args) throws MalformedURLException{
        new DesiredCapabilities();
            URL serverurl = new URL("http://localhost:9515");
            DesiredCapabilities capabilities = DesiredCapabilities.chrome();
            WebDriver driver = new RemoteWebDriver(serverurl,capabilities);
        driver.get("http://www.google.com");
        WebElement searchEdit = driver.findElement(By.name("q"));
        searchEdit.sendKeys("Selftechy on google");
        searchEdit.submit();

    }
}
Now, try to execute the code by clicking Run As –> JUnit Test.  It should be executing the test to the completion.


