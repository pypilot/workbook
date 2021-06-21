On the openplotter pypilot User Interface screen, there are two more buttons ‘Browser Control’, and ‘Open’. 'Browser Control' is a toggle, you can switch it on, and it enables the browser interface. 

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/17980560/110981380-5a9c6080-8367-11eb-8cf5-785f5263cb42.png"/></p>

When you click 'Open', it starts a browser on http://localhost:8080, which is where the browser interface sits.

> Note: on Tinypilot, the interface sits at port :80

In fact, you can also open the browser interface from the machine where you are running your VNC client on. Just type http://10.10.10.1:8080/ and you see the browser interface. 
 
<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/17980560/110981510-8881a500-8367-11eb-8679-792757488941.png"/></p>
 
At the moment of writing, it shows pypilot Server: Not Connected. This seems to be related to a library incompatibity that can be solved with the following workaround:
```
sudo pip3 uninstall python-socketio python-engineio flask-socketio
sudo pip3 install "flask-socketio<5" "python-socketio<5"
```

> Note: if you run pypilot_web [at the prompt](https://github.com/pypilotWorkbook/workbook/wiki/Step-8-Looking-under-the-hood-of-openplotter#running-pypilot-at-the-prompt), for debugging purposes, it serves at port 8000 instead of 8080, so you need to type http://10.10.10.1:8000.

***
[Step 5: The HAT interface >>>](Step-5-The-HAT-interface)