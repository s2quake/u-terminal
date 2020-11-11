---
layout: page
permalink: /docs
permalink_name: /docs
title: Document
detail_image: assets/images/docs-logo.png
---

# /Naming convention

Kebab case

* Exit -> exit
* GameObject -> game-object

# /Base definitions

base command

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

base async command

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

base sub command

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

# /Property definitions

## 1. CommandPropertyAttribute

* The default format is '\-\-Property Value'
* If **'DefaultValue'** of Attribute, the value can be omitted. However, the delimiter must be specified. like '\-\-Property'
* All properties are set to their initial values. If you want to specify an initial value, specify **'InitValue'** of Attribute.

```cs
[CommandProperty]
public string Value { get; set; }

[CommandProperty(DefaultValue = "text")]
public string Value { get; set; }

[CommandProperty(InitValue = "")]
public string Value { get; set; }
```

## 2. CommandPropertyRequiredAttribute

* All properties specify only values without delimiters.
* If **'DefaultValue'** of Attribute, the value can be omitted
* If **'IsExplicit'** or Attribute is specified as true, you must specify a delimiter and a value. like '\-\-Property Value'.

```cs
[CommandPropertyRequired]
public string Value { get; set; }

[CommandPropertyRequired(DefaultValue = "text")]
public string Value { get; set; }

[CommandPropertyRequired(IsExplicit = true)]
public string Value { get; set; }
```

## 3. CommandPropertySwitchAttribute

* Only available for Boolean type properties.
* If the delimiter is specified, the value of the property is true, otherwise the value is false.

```cs
[CommandPropertySwitch]
public bool Value { get; set; }
```

## 4. CommandPropertyArrayAttribute

* Only available for Array type properties.
* All properties specify only values without delimiters.
* CommandPropertyArrayAttribute cannot be multiple in one command.

```cs
[CommandPropertyArray]
public string[] Values { get; set; }
```
