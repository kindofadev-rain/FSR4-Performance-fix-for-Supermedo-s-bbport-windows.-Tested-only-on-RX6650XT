# FSR 4 fix for Supermedo's Bloodborne Windows Port.
##Tested on RDNA2.

FSR 4 was running at around 20 fps for me on an RX 6650 XT, while FSR 3.1 was doing 100+. The port has an option to use the original FSR 4 shaders instead of its optimized ones, and turning that on fixed it.

My setup:AMD Rryzen 5 5600x,XFX RX 6650 XT, RAM 16gb 3200mhz, AMD driver 26.8.1.

What I get now:

- FSR 3.1 Quality: ~100-110 fps
- FSR 4 Ultra Performance, default: ~20 fps
- FSR 4 Ultra Performance with the fix: ~76 fps
- FSR 4 Balanced with the fix: ~66 fps

I've only tried it on my own card, so I don't know if it helps other RDNA2 GPUs. I also haven't compared image quality, so it might look a bit different. If you notice anything, let me know.

## How to do it

1. Close the game and the launcher.
2. Right-click inside the `bbport-windows` folder and click "Open in Terminal".
3. Run this:

   ```
   setx BB_FSR4_OPT 0
   ```

4. Open the launcher again and pick FSR 4.

It stays set after a reboot. If you want to check, open a new PowerShell window and run `echo $env:BB_FSR4_OPT`. It should print 0.

If you only want to try it once, run this from the bbport-windows folder instead:

```
$env:BB_FSR4_OPT = "0"
.\Bloodborne.exe --play
```

## Undo

```
[Environment]::SetEnvironmentVariable("BB_FSR4_OPT", $null, "User")
```

or just delete it from "Edit environment variables for your account" in Windows.

## One more thing

FSR 4.1.1 doesn't work on my card. It needs the Vulkan extension `VK_VALVE_shader_mixed_float_dot_product` and the AMD driver doesn't expose it. You can check yours with `vulkaninfo | findstr /i VK_VALVE` after installing the [Vulkan SDK](https://vulkan.lunarg.com/sdk/home).
