https://tecadmin.net/google-chrome-headless-features/

To Use Google Chrome Headless Features

Google Chrome latest version released with a new useful feature Headless Chrome. The headless Chrome is useful for browser automation. You can capture screenshots of any web page using the command line as well as programming language without starting Chrome GUI. It also supports to print the web page DOM and create a pdf of the web page. This tutorial will help you Use Google Chrome Headless Features on Linux command line.


1. Starting Headless Chrome
Open the system console and start Google Chrome headless more using --headless command line option.

$ google-chrome --headless http://www.example.com
This headless mode also supports the remote debugging option to check what’s happening. You can access the system on specified port in any other browser and check what is rendering there. Start the debugging with the following command on specified port:

$ google-chrome --headless  --remote-debugging-port=9222 https://google.com
Now visit http://localhost:9222 in another web browser.



The headless Chrome also has many other useful features like printing the DOM, capture screenshot or create pdf of any web page through the command line.

2. Capture Webpage Screenshot
You can use --screenshot option to capture screenshot of any web page. The output screenshot will be saved in current directory. For more details visit here.

$ goolge-chrome --headless --disable-gpu --screenshot http://www.example.com/
3. Create Webpage PDF
You can use --print-to-pdf option to create PDF of any web page. The output pdf file will be saved in current directory. For more details visit here.

$ google-chrome --headless --disable-gpu --print-to-pdf http://www.example.com/
4. Print the Webpage DOM
You can use --dump-dom flag to print document.body.innerHTML to standard output.
The –dump-dom flag prints document.body.innerHTML to stdout:

$ google-chrome --headless --disable-gpu --dump-dom http://www.example.com/
Reference: visit following link to know more details about headless Chrome.

https://developers.google.com/web/updates/2017/04/headless-chrome


