---
title: Configure and Run Scripts in VS Code
categories: Python
tags: 
    - Visual Studio Code 
---

How to set up VS Code to execute a Python script...

### Script
```python
# TODO: write script
```

### Setup
1. Download and install [Visual Studio Code](https://code.visualstudio.com/) if needed.
1. Install the [Python extension](https://marketplace.visualstudio.com/items?itemName=donjayamanne.python):

    ![Install Python extension]({{ "/images/20171020/add-py-ext.PNG" | absolute.url }})
1. Open the directory containing your script(s).
1. Open the Debug pane and click the gear button to edit the launch.json configuration file:

    ![launch-json]({{ "/images/20171020/launch-json.png" | absolute.url }})
1. Click the big blue **Add Configuration..** button in the bottom right of launch.json:

    ![configuration]({{ "/images/20171020/config.png" | absolute.url }})
1. Create a new Python configureation object in launch.json:

    ![Add Python configuration Image]({{ "/images/20171020/add-config.png" | absolute.url }})
1. Modify the configuration
    ```json
    {
        "name": "print_cmd_params",
        "type": "python",
        "request": "launch",
        "stopOnEntry": true,
        "pythonPath": "${config:python.pythonPath}",
        "program": "${file}",
        "cwd": "${workspaceRoot}",
        "env": {},
        "envFile": "${workspaceRoot}/.env",
        "debugOptions": [
            "WaitOnAbnormalExit",
            "WaitOnNormalExit",
            "RedirectOutput"
        ],
        "args": [
            "Successfully",
            "executed",
            "script"
        ]
    }
    ```
1. Select the configuration and run:  

    ![Select and run]({{ "/images/20171020/run-script.png" | absolute.url }})
1. Review the Debug console.

### References

[Stack Overflow](https://stackoverflow.com/questions/29987840/how-to-execute-python-code-from-within-visual-studio-code)  
[VS Code Debugging](https://code.visualstudio.com/docs/editor/debugging#_launch-configurations)  
[Python Extension](https://github.com/DonJayamanne/pythonVSCode/wiki/Debugging)  