# PSMenu

Simple module to generate interactive console menus (like yeoman)

# Examples

Very basic example.

```powershell
Show-Menu @("option 1", "option 2", "option 3")
```

![Basic example](./docs/basic-example.gif)

## Custom formatting

Custom formatting of menu items and multi-selection:

```powershell
Show-Menu -MenuItems $(Get-NetAdapter) -MultiSelect -MenuItemFormatter { $Args | Select -Exp Name }
```

![Classes as options (complex example)](./docs/classes-as-options.gif)

## Custom options

You can also use custom options (enriched options). A more complicated example:

```powershell
class MyMenuOption {
    [String]$DisplayName
    [ScriptBlock]$Script

    [String]ToString() {
        Return $This.DisplayName
    }
}

function New-MenuItem([String]$DisplayName, [ScriptBlock]$Script) {
    $MenuItem = [MyMenuOption]::new()
    $MenuItem.DisplayName = $DisplayName
    $MenuItem.Script = $Script
    Return $MenuItem
}

$Opts = @(
    $(New-MenuItem -DisplayName "Say Hello" -Script { Write-Host "Hello!" }),
    $(New-MenuItem -DisplayName "Say Bye!" -Script { Write-Host "Bye!" })
)

$Chosen = Show-Menu -MenuItems $Opts

& $Chosen.Script
```

This will show the menu items like you expect.

![Custom formatters and multiselect](./docs/custom-formatter.gif)

## Separators

```powershell
Show-Menu @("Option A", "Option B", $(Get-MenuSeparator), "Quit")
```

Separators are unselectable items used for visual distinction in the menu.

![Separator support](./docs/separator-support.gif)

# Installation

You can install it from the PowerShellGallery using PowerShellGet

```powershell
Install-Module PSMenu
```

# Features

- Returns value of selected menu item
- Returns index of selected menu item (using `-ReturnIndex` switch)
- Multi-selection support (using `-MultiSelect` switch), use `spacebar` to select items
- Navigation with `up/down/page-up/page-down/home/end` keys
- Longer list scroll within window
- Support for separators
- Esc key quits the menu (`$null` returned)
- Extensively documented

# Documentation

For details, check out the comment-based help in [the source code](./PSMenu/Public/Show-Menu.ps1),
or use `Get-Help Show-Menu` from the command-line. See also [`Get-Help`](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/get-help?view=powershell-5.1).

# Contributing

- Source hosted at [GitHub](https://github.com/Sebazzz/PSMenu)
- Report issues/questions/feature requests on [GitHub Issues](https://github.com/Sebazzz/PSMenu/issues)

Pull requests are very welcome!
