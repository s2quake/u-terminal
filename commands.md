---
layout: page
permalink: /commands
permalink_name: /commands
title: Commands
detail_image: assets/images/docs-logo.png
---

# Table of contents

[1.Naming convention](#naming-convention)
[2.Base definitions](#base-definitions)
[3.String output & progress](#string-output-and-progress)
[4.Property definitions](#property-definitions)
[5.Command enabled or disabled](#command-enabled-or-disabled)
[6.Autocomplete](#autocomplete)
[7.Register command](#register-command)
[8.Register configuration](#register-configuration)

***

## /Naming convention

Kebab case

### 1. Command Name

* class RestartCommand -> 'restart'
* class GameObjectCommand -> 'game-object'

### 2. SubCommand Name

* void Show() -> 'show'
* Task DownloadAsync() -> 'download'
* void ShowList() -> 'show-list'

### 3. Property Name

* string Filter -> 'filter'
* bool IsRecursive -> 'is-recursive'
* int ExitCode -> 'exit-code'

***

## /Base definitions

### 1. base command

```cs
public class BaseCommand : TerminalCommandBase
{
    public BaseCommand(ITerminal terminal)
        : base(terminal)
    {
    }

    [CommandPropertyRequired]
    public string RequiedValue { get; set; }

    [CommandProperty]
    public string OptionValue { get; set; }

    [CommandPropertySwitch]
    public bool SwitchValue { get; set; }

    [CommandPropertyArray]
    public string[] Values { get; set; }

    protected override void OnExecute()
    {
    }
}
```

```bash
base value --option-value v --switch-value value1 value2 value3
```

### 2. base async command
```cs
class BaseAsyncCommand : TerminalCommandAsyncBase
{
    public BaseAsyncCommand(ITerminal terminal)
        : base(terminal)
    {
    }

    [CommandProperty]
    public string Value1 { get; set; }

    protected override Task OnExecuteAsync(CancellationToken cancellation)
    {
    }
}
```

```bash
base-async --value1 v
```

### 3. base sub command

```cs
public class BaseSubCommand : TerminalCommandMethodBase
{
    public BaseSubCommand(ITerminal terminal)
        : base(terminal})
    {
    }

    [CommandMethod]
    public void Method1(string value1, string value2 = "")
    {
    }

    [CommandMethod]
    [CommandMethodProperty(nameof(PropertyValue))]
    public void Method2(string value1, params string[] args)
    {
    }

    [CommandMethod]
    public Task Method3Async(CancellationToken cancellation, string value1)
    {
    }

    [CommandProperty]
    public string PropertyValue
    {
        get; set;
    }
```

```bash
base-sub method1 value
base-sub method2 value v1 v2 v3 --property-value p
base-sub method3 value
```

***

## /String output and progress

### 1. Write string

WriteLineAsync must be used in asynchronous methods.

```cs
...
    protected override void OnExecute()
    {
        this.WriteLine("tex");
        this.WriteLine("foreground color", TerminalColor.Red);
}
...

...
    protected override async Task OnExecuteAsync(CancellationToken cancellation)
    {
        await this.WriteLineAsync("tex");
        await this.WriteLineAsync("foreground and background color", TerminalColor.Red, TerminalColor.White);
    }
...
```

### 2. Write many strings

Writing many strings one by one can slow down the speed, so using StringBuilder is a good idea to write them all at once.

```cs
...
    protected override void OnExecute()
    {
        var sb = new StringBuilder();
        for (var i = 0; i < 1000; i++)
        {
            sb.AppendLine($"index: {i}");
        }
        this.Write(sb.ToString());
    }
...
```

### 3. Dispatcher.InvokeAsync

You can invoke the method from the main thread using the dispatcher in an asynchronous method.

```cs
...
    protected override async Task OnExecuteAsync(CancellationToken cancellation)
    {
        this.Dispatcher.InvokeAsync(() => Debug.Log("log"));
    }
...
```

### 4. Progress

<img id="body-img" src="./assets/images/progress.gif" alt="progress" />

```cs
...
    protected override async Task OnExecuteAsync(CancellationToken cancellation)
    {
        await this.WriteLineAsync("Start Loading.", TerminalColor.Red);
        for (var i = 0; i < 100; i++)
        {
            if (cancellation.IsCancellationRequested == true)
            {
                throw new Exception("Loading canceled.");
            }
            await this.SetProgressAsync($"{i}%", (float)i / 100);
        }
        await this.SetProgressAsync($"{100}%", 1.0f);
        await this.CompleteProgressAsync("Completed");
    }
...
```

***

## /Property definitions

### 1. CommandPropertyAttribute

* The default format is '\-\-Property Value'
* If **'DefaultValue'** of Attribute is specified, the value can be omitted. However, the delimiter must be specified. like '\-\-Property'
* All properties are set to their initial values. If you want to specify an initial value, specify **'InitValue'** of Attribute.

```cs
[CommandProperty]
public string Value { get; set; }

[CommandProperty(DefaultValue = "text")]
public string Value { get; set; }

[CommandProperty(InitValue = "")]
public string Value { get; set; }
```

### 2. CommandPropertyRequiredAttribute

* All properties specify only values without delimiters.
* If **'DefaultValue'** of Attribute is specified, the value can be omitted
* If **'IsExplicit'** or Attribute is specified as true, you must specify a delimiter and a value. like '\-\-Property Value'.

```cs
[CommandPropertyRequired]
public string Value { get; set; }

[CommandPropertyRequired(DefaultValue = "text")]
public string Value { get; set; }

[CommandPropertyRequired(IsExplicit = true)]
public string Value { get; set; }
```

### 3. CommandPropertySwitchAttribute

* Only available for Boolean type properties.
* If the delimiter is specified, the value of the property is true, otherwise the value is false.

```cs
[CommandPropertySwitch]
public bool Value { get; set; }
```

### 4. CommandPropertyArrayAttribute

* Only available for Array type properties.
* All properties specify only values without delimiters.
* CommandPropertyArrayAttribute cannot be multiple in one command.

```cs
[CommandPropertyArray]
public string[] Values { get; set; }
```

***

## /Command enabled or disabled

### 1. Command

Override IsEnabled Property

```cs
public class BaseCommand : TerminalCommandBase
{
    ...
    public override bool IsEnabled => true;
    ...
}
```

### 2. SubCommand

Use property in Can + "name" format

```cs
public class BaseSubCommand : TerminalCommandMethodBase
{
    [CommandMethod]
    public void Method1(string value1, string value2 = "")
    {
    }

    public bool CanMethod1 => true;

    [CommandMethod]
    public Task Method2Async(CancellationToken cancellation, string value1)
    {
    }

     public bool CanMethod2 => true;
}
```

***

## /Autocomplete

### 1. Command

Override GetCompletions Method

```cs
public class BaseCommand : TerminalCommandBase
{
    public override string[] GetCompletions(CommandCompletionContext completionContext)
    {
        var memberDescriptor = completionContext.MemberDescriptor;
        if (memberDescriptor != null && memberDescriptor.DescriptorName == nameof(Value))
        {
            var items = new string[] { "a", "b", "c" };
            var query = from item in items
                        where item.StartsWith(completionContext.Find)
                        select item;
            return query.ToArray();
        }
        return null;
    }

    [CommandProperty]
    public string Value { get; set; }
}
```

### 2. SubCommand

Override GetCompletions Method

```cs
public class BaseSubCommand : TerminalCommandMethodBase
{
    public override string[] GetCompletions(CommandMethodDescriptor methodDescriptor, CommandMemberDescriptor memberDescriptor, string find)
    {
    }
}
```

 or Define method named Complete + "name"

```cs
public class BaseSubCommand : TerminalCommandMethodBase
{
    [CommandMethod]
    public void Method1(string value1, string value2 = "")
    {
    }

    public string[] CompleteMethod1(CommandMemberDescriptor descriptor, string find)
    {
        ...
    }
}
```

***

## /Register command

Add the CommandSystem component to a terminal object with the CommandContextHost component.

```cs
public class CommandSystem : CommandSystemBase
{
    protected override IEnumerable<ICommand> OnCommandProvide(ITerminal terminal)
    {
        yield return new BaseCommand(terminal);
    }
}
```
***

## /Register configuration

You can specify values through the config command in the terminal by registering fields and properties.

### 1. Command configuration

To register a static field or property

```cs
public class CommandSystem : CommandSystemBase
{
    protected override void Awake()
    {
        base.Awake();
        AddConfig(new CommandConfiguration<bool>("sound.pause", (o) => AudioListener.pause, (o, v) => AudioListener.pause = v));
        AddConfig(new CommandConfiguration<float>("sound.volume", (o) => AudioListener.volume, (o, v) => AudioListener.volume = v));
    }

    protected override void OnDestroy()
    {
        base.OnDestroy();
        RemoveConfig("sound.pause");
        RemoveConfig("sound.volume");
    }
}
```

### 2. Field or Property configuration

To register fields or properties for a specific instance

```cs
public class TestConfiguration : MonoBehaviour
{
    [SerializeField]
    private float value = 0;
    [SerializeField]
    private CommandSystem commandSystem = null;

    public bool IsValue { get; set; }

    private void Awake()
    {
        if (this.commandSystem != null)
        {
            this.commandSystem.AddConfig(new FieldConfiguration("test.value", this, nameof(value)));
            this.commandSystem.AddConfig(new PropertyConfiguration("test.IsValue", this, nameof(IsValue)));
        }
    }

    private void OnDestroy()
    {
        if (this.commandSystem != null)
        {
            this.commandSystem.RemoveConfig("test.value");
            this.commandSystem.RemoveConfig("test.IsValue");
        }
    }
}
```

## 3. Instance configuration

You can use the CommandConfigurationAttribute to register multiple configurations at once.

```cs
[CommandConfiguration("test")]
public class TestController : MonoBehaviour
{
    public CommandSystem commandSystem = null;

    [Range(0, 10)]
    [Tooltip("integer value")]
    [CommandConfiguration]
    public int value = 0;

    private TestSettings settings;

    private void Awake()
    {
        this.settings = new TestSettings();
        if (this.commandSystem != null)
        {
            this.commandSystem.AddConfigs(this);
            this.commandSystem.AddConfigs(this.settings, "settings");
        }
    }

    private void OnDestroy()
    {
        if (this.commandSystem != null)
        {
            this.commandSystem.RemoveConfigs(this);
            this.commandSystem.RemoveConfigs(this.settings, "settings");
        }
        this.settings = null;
    }

    [CommandConfiguration]
    public class TestSettings
    {
        [CommandConfiguration]
        public int value = 0;
    }
}
```

## 4. Usage in terminal

Show list of configurations
```bash
config --list
```

Show value of 'test.value' configuration
```bash
config test.value
```

Set value of 'test.value' configuration
```bash
config test.value 0
```

Reset value of 'test.value' configuration
```bash
config test.value --reset
```
