# Prompt the user to select from a list

## PowerShell

Use the built-in function `$Host.UI.PromptForChoice`

### Arguments

`Caption` the caption preceding the prompt text
`Prompt` the prompt text
`Options` the available choices, `Collection<ChoiceDescription>`
`Default` the index of the choice selected by default

The `Options` each contain a `Label` and `HelpMessage` property, but can also be implicitly cast from a string if no help message is required.  The `Label` supports using the `&` character to indicate a hotkey.

### Examples

Options defined using a string array, with hotkeys and no default selection.

The `[?] Help` option will not display any useful information.

```powershell
$Caption = 'Fruit'
$Prompt = 'What fruit do you want to eat?'
$Options = @('&apple', '&banana', '&melon', '&peach')
$Host.UI.PromptForChoice($Caption, $Prompt, $Options, -1)

# Fruit
# What fruit do you want to eat?
# [A] apple  [B] banana  [M] melon  [P] peach  [?] Help
```

Options defined using a `Collection<ChoiceDescription>` to include helpful text, with the first option being the default.

```powershell
using namespace System.Collections.ObjectModel
using namespace System.Management.Automation.Host

$Caption = 'Sorcerer Class'
$Prompt = 'What kind of sorcerer do you want to be?'
$Options = [Collection[ChoiceDescription]]@()
$Options.Add([ChoiceDescription]::new('&red', 'a master of fire spells'))
$Options.Add([ChoiceDescription]::new('&blue', 'a powerful frost magician'))
$Options.Add([ChoiceDescription]::new('&grey', 'a fearsome necromancer'))

$Host.UI.PromptForChoice($Caption, $Prompt, $Options, 0)

# Sorcerer Class
# What kind of sorcerer do you want to be?
# [R] red  [B] blue  [G] grey  [?] Help (default is "R"): ?
# R - a master of fire spells
# B - a powerful frost magician
# G - a fearsome necromancer
```
