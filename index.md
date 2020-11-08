---
layout: home
permalink: /
permalink_name: /home
title: Home
---

# 소개

u-Terminal 은 유닉스의 터미널, 윈도우의 콘솔, FPS 게임의 콘솔창처럼

사용자의 입력을 받고 결과를 출력하는 REPL 환경을 제공합니다.

# 유니티 버전

2018.4.27f1 and later

# 사용 예제

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

# 명령어 정의

* 비동기 명령 지원
* 하위 명령어 지원
* 명령어 자동완성
* 도움말 자동 생성

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

![restart-command](./assets/images/restart-command.gif){: width="700" height="438"}


# 속성 설정

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

![config-command](./assets/images/config-command.gif){: width="700" height="438"}


# show/hide terminal

default key : ctrl + `

![visible-controller](./assets/images/visible-controller.gif){: width="700" height="438"}


# javascript

*javascript content is not included in the asset.*

The figure below is implemented using the [jint](https://github.com/sebastienros/jint) library.

![visible-controller](./assets/images/javascript.gif){: width="700" height="438"}
