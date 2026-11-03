# Razer Huntsman Mini – ESC Key Remap

A simple Windows registry fix for the **Razer Huntsman Mini Nordic (RZ03-03390)** that swaps the default ESC and ½ key outputs.

No Razer Synapse, third-party software, or background processes required.

## What it does

| Key combination | Output |
|---|---|
| `ESC` | `ESC` |
| `FN + ESC` | `½` |

## Installation

1. Open **PowerShell as Administrator**.
2. Copy and paste the following command:

```powershell
$path = 'HKLM:\SYSTEM\CurrentControlSet\Control\Keyboard Layout'

# Check for an existing remapping
if (Get-ItemProperty -Path $path -Name 'Scancode Map' -ErrorAction SilentlyContinue) {
    throw 'An existing Scancode Map was found. Back it up before proceeding.'
}

# Swap Escape (0x01) and Danish ½ (0x29)
[byte[]]$map = @(
    0,0,0,0,
    0,0,0,0,
    3,0,0,0,
    1,0,41,0,
    41,0,1,0,
    0,0,0,0
)

New-ItemProperty -Path $path `
    -Name 'Scancode Map' `
    -PropertyType Binary `
    -Value $map `
    -Force

Write-Host 'Remapping installed. Restart Windows.'
```

3. Restart Windows.

That's it!

## Uninstallation

To restore the original key behaviour, open PowerShell as Administrator and run:

```powershell
Remove-ItemProperty `
    -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Keyboard Layout' `
    -Name 'Scancode Map' `
    -ErrorAction SilentlyContinue
```

Restart Windows afterwards.

## Compatibility

- **Keyboard:** Razer Huntsman Mini
- **Model:** RZ03-03390
- **Layout:** Nordic / Danish
- **OS:** Windows 10 / Windows 11

## Notes

- No additional software required.
- No Synapse installation required.
- The remapping persists across restarts.
- This is a Windows-level remapping, not a keyboard firmware modification.
- The mapping affects all keyboards connected to Windows, not just the Huntsman Mini.

*Tested successfully on the Razer Huntsman Mini Nordic edition.*
