# Fix: very low FPS with FSR 4 on AMD RDNA2 (Windows)

If FSR 4 runs far slower than FSR 3.1 on your AMD RDNA2 card (RX 6000 series), setting one environment variable can fix it.

## Report

| | |
|---|---|
| GPU | AMD Radeon RX 6650 XT |
| OS | Windows |
| Driver | AMD proprietary 26.8.1 (Vulkan 1.4.315) |
| Port | bbport Windows build |

| Setting | FPS |
|---|---|
| FSR 3.1, Quality | ~100-110 |
| FSR 4, Ultra Performance (default) | ~20 |
| FSR 4, Ultra Performance, `BB_FSR4_OPT=0` | ~76 |
| FSR 4, Balanced, `BB_FSR4_OPT=0` | ~66 |

Only tested on one card (RX 6650 XT). It may help other RDNA2 cards, but this is not confirmed.

## What it does

`BB_FSR4_OPT=0` makes the port use the original FSR 4 shaders instead of the optimized ones it normally uses (`BB_FSR4_OPT` is a documented option of bbport). On the tested setup the optimized shaders were the cause of the slowdown.

Image quality with the original shaders has not been compared. If you notice a difference, please report it.

## How to apply it

1. Close the game and the launcher if they are open.
2. Right-click inside the `bbport-windows` folder and choose **Open in Terminal**.
3. Run:

   ```
   setx BB_FSR4_OPT 0
   ```

4. Start the launcher again and select FSR 4.

`setx` saves the variable for your Windows user. It stays after closing programs and rebooting, and it only affects programs started after the command.

## Check that it is set

Open a **new** PowerShell window and run:

```
echo $env:BB_FSR4_OPT
```

It should print `0`.

## Try it for one session only

In PowerShell, from the `bbport-windows` folder:

```
$env:BB_FSR4_OPT = "0"
.\Bloodborne.exe --play
```

The variable only lasts for that PowerShell window.

## Undo

```
[Environment]::SetEnvironmentVariable("BB_FSR4_OPT", $null, "User")
```

Or remove it from **Edit environment variables for your account** in Windows.

## Notes

- FSR 4.1.1 needs the Vulkan extension `VK_VALVE_shader_mixed_float_dot_product`. The AMD proprietary driver 26.8.1 on the RX 6650 XT does not expose it, so FSR 4.1.1 is not available there. FSR 4 (v07 INT8) and FSR 3.1 work.
- To check: `vulkaninfo | findstr /i VK_VALVE`
