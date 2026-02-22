# PG39WCDM-ICC-HDR-MITIGATION

Custom ICC profile to mitigate the ASUS SWIFT PG39WCDM HDR related issues.

## Background

My beloved PG35VQ died a month before the warranty expired. ASUS sent me a PG39WCDM as a warranty replacement. Having become accustomed to the quality of a Mini-LED monitor, the transition to a WOLED highlighted the stark downgrade in color quality in this newer display. It felt wasteful to not try and make it work, given I was sent the thing for free, so I have spent the last 6 months perfecting this custom profile that goes a long way to mitigating the worst of the issues this display encounters when viewing HDR content, and particularly SDR content when HDR is enabled. Fortunately, I'm a professional graphics engineer with too much free time on my hands!

## What It Does

The ICC profile contains a custom LUT that adjusts the gamma curve and combats desaturation in low luminance levels through progressive gamut expansion.

### Corrections Applied

- Customized gamma curve to mitigate the display's effective gamma being closer to 1.8 than the expected 2.2
- Custom handling so both PQ and Gamma content maintains a smooth curve through the toe (below 1 nit)
- Custom Rec.2020 primaries adapted to the device's native color characteristics
- Default values set to optimal for the monitor
- Fixed MaxFALL to 265
- MaxCLL to 1300, MinCLL 0.00248 (lowest value the monitor can differentiate)
- Contrast boost to 125% (corrects the 'washed-out' look and brings contrast in line with SDR mode)
- Saturation adjustment to 98% (for primary accuracy)

## Installation

### 1. Install the ICC Profile

1. Copy `PG39WCDM_HDR_Correction.icm` to `C:\Windows\system32\spool\drivers\color` or right-click the file and select **Install Profile**.
2. Open **Windows Settings > Display > Color Management** and set the profile as the default **HDR** profile for your PG39WCDM display.

### 2. Ensure the Profile Applies at Boot

Windows 11 has a bug where the MHC2 calibration pipeline fails to activate at startup. Use [ApplyIccLut](https://github.com/ninlilizi/ApplyIccLut) to enable the Windows automatic colour management machinery and force the GPU driver to apply the profile:

1. Download or build [ApplyIccLut](https://github.com/ninlilizi/ApplyIccLut) and run it once (it will request administrator privileges):
   ```
   ApplyIccLut.exe
   ```
   This re-applies the default profiles for all monitors and enables the Windows calibration management subsystem, which should cause the profiles to **persist across subsequent reboots** without needing to run the tool again.

2. If profiles still fail to apply after a reboot, you can add the tool as a Task Scheduler fallback:
   - Open **Task Scheduler** and select **Create Task**.
   - **General** tab: Check **"Run with highest privileges"** and ensure **"Run only when user is logged on"**.
   - **Trigger** tab: **"At log on"** for your user account (optionally add a 10-second delay).
   - **Action** tab: **"Start a program"** with the full path to `ApplyIccLut.exe` (no arguments needed).

### 3. Recommended Display Settings

**Nvidia Control Panel:**
- Brightness: 50%
- Contrast: 50%
- Digital Vibrance: 50% (higher values reduce colour accuracy)

**Windows Settings:**
- SDR content brightness: 20-40 (personal preference; I use 40)

## Included Files

| File | Description |
|---|---|
| `PG39WCDM_HDR_Correction.icm` | ICC profile for Windows Color Management |
| `PG39WCDM_HDR_Correction_SDR.cube` | SDR correction LUT (Rec.709 primaries) |
| `PG39WCDM_HDR_Correction_HDR.cube` | HDR correction LUT (Rec.2020 primaries) |
| `PG39WCDM_LUT-s16.png` | 16x 3D correction LUT (PNG) |
| `PG39WCDM_LUT-s32.png` | 32x 3D correction LUT (PNG) |
| `PG39WCDM_LUT-s16.dds` | 16x 3D correction LUT (DDS, BC7, no mips) |
| `PG39WCDM_LUT-s32.dds` | 32x 3D correction LUT (DDS, BC7, no mips) |

### Usage Notes

- The **PNG LUTs** can be used with ReShade's 'LUT' basic shader if you want to correct only a single SDR game.
- The **.cube files** are compatible with professional colour pipelines and software. They serve niche needs for users who know how to use them.
- Apply the LUT at **either** system level or in an application. Applying it at both levels will over-correct.

## Related Projects

- [ColorControl-PG39WCDM](https://github.com/ninlilizi/ColorControl-PG39WCDM) - Customized fork of ColorControl used to generate this profile
- [PqLumaCurveTestBars](https://github.com/ninlilizi/PqLumaCurveTestBars) - Tool for visualising the accuracy of the PQ curve
- [ApplyIccLut](https://github.com/ninlilizi/ApplyIccLut) - Tool to force-apply ICC profiles at boot on Windows 11

## License

MIT

