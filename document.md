---
layout: page
permalink: /docs
permalink_name: /docs
title: Document
detail_image: assets/images/docs-logo.png
---

# Table of contents

[1.Naming convention](#naming-convention)
[2.Base definitions](#base-definitions)
[3.Property definitions](#property-definitions)
[4.Command enabled or disabled](#command-enabled-or-disabled)
[5.Autocomplete](#autocomplete)

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

```
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

```
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

```
base-sub method1 value
base-sub method2 value v1 v2 v3 --property-value p
base-sub method3 value
```

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

## /Command enabled or disabled

### 1. Command

Override IsEnabled Property

```cs
public class BaseCommand : TerminalCommandBase
{
    ...
    public override bool IsEnabled => base.IsEnabled;
    ...
}
```

### 2. SubCommand

Use properties in Can + "name" format

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
