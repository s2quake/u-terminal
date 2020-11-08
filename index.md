---
layout: home
permalink: /
permalink_name: /home
title: Home
detail_image: assets/images/logo.png
---

# Summary

u-Terminal is like a terminal in Unix, a console in Windows, and a console window in FPS games.

Provides a REPL environment for receiving user input and outputting results.

# Unity Version

2018.4.27f1 and later

# Usage

```
comp ls /GameObject
config audio.pause true
date --locale ko-KR --utc
obj ls / --recursive
exit
info productName
resolution --full
scene --list
version
```

# Command

* Asynchronous command support
* Subcommand support
* Autocomplete command
* Automatically generate help

```cs
[CommandSummary("Exit the application.")]
public class TestExitCommand : TerminalCommandBase
{
    public TestExitCommand(ITerminal terminal)
        : base(terminal)
    {
    }

    [CommandPropertyRequired(DefaultValue = 0)]
    [CommandSummary("Specifies the exit code. The default is 0.")]
    public int ExitCode { get; set; }

    protected override void OnExecute()
    {
#if UNITY_EDITOR
        UnityEditor.EditorApplication.isPlaying = false;
#else
        UnityEngine.Application.Quit();
#endif
    }
}
```

<img id="body-img" src="./assets/images/restart-command.gif" alt="restart-command" />

# Configuration

```cs
public class TestConfiguration : MonoBehaviour
{
    [SerializeField]
    private float value = 0;
    [SerializeField]
    private ConfigurationSystem configSystem = null;

    private void Awake()
    {
        if (this.configSystem != null)
        {
            this.configSystem.AddConfig(new FieldConfiguration("test.value", this, nameof(value)) { DefaultValue = this.value });
        }
    }
}
```

<img id="body-img" src="./assets/images/config-command.gif" alt="config-command" />


# show/hide terminal

default key : ctrl + `

<img id="body-img" src="./assets/images/visible-controller.gif" alt="visible-controller" />

# javascript

*javascript content is not included in the asset.*

The figure below is implemented using the [jint](https://github.com/sebastienros/jint) library.

<img id="body-img" src="./assets/images/javascript.gif" alt="javascript" />
