# PG39WCDM-ICC-HDR-MITIGATION
Custom ICC profile to mitigate the ASUS SWIFT PG39WCDM HDR related issues.

Why does this exist? My beloved PG35VQ died a month before the warrenty expired. ASUS sent me a PG39WCDM as a warrenty replacement. Having become accustomed the the quality of a Mini-LED monitor, the transition to a WOLED highligted the stark downgrade in color quality in this newer display. It felt wasteful to not try and make it work, given I was sent the thing for free, so I have spent the last month perfecting this custom profile that goes a long way to mitigating the worst of the issues this display encounters when viewing HDR content, and particularly SDR content when HDR is enabled. Fortunately, I'm a professional graphics engineer with too much free time on my hands!

One cavaet, is the LUT that performs most of the lifting doesn't apply automatically on boot, or resume from standby. But opening the Color applet from the control panel, selecting the install profile and clicking the 'Set Default' button, even though it is already the default will immediately apply the correction LUT which performs the actual work. It's suboptimal to apply this by hand every time, but you can set an Event task to do it for you are boot, if desired. There are also tools that enforce it will running/resuming/etc. I would link to that tool, but it currently doesn't work in the latest version of Windows11, so will have to live with manual application until thast project is updated by it's owner.

The ICC profile contains a custom LUT that adjust the gamma curve and combats desaturation in low luminance levels through progressive gamut expansion.

* For color accuracy, set Digital Vibrance to 48%.
* For the most punch. set Digital Vibrance to 55%.
* values outside of that range will mess up the color accuracy across a bunch of luminance levels, so treat that as your safe range.

Additionally:

* Customized gamma curve to mitigate the displays effective gamma being closer to 1.8 than the expected 2.2.
* Upgrade profile version to MHC3 to support HDR specific parameters.
* Default values set to optimal for the monitor.
* Fixed MaxFALL to 265.
* MaxCLL to 1000, MinCLL 0.0007.
* Saturation boost to 15%.
* Output custom .cube 3D LUTs with maximum number of steps to allow color accurate correction in professional softwares.

The profiles were generated using a customized fork of ColorControl. If you wish to make your own iteration or examine the changes put into this custom profile, it is here: https://github.com/ninlilizi/ColorControl-PG39WCDM

Contains:
* PG39WCDM_HDR_Tonemaped_Gamma.icm - ICC profile
* PG39WCDM_HDR_Tonemaped_Gamma_SDR.cube - SDR correction LUT (Rec.709 primaries)
* PG39WCDM_HDR_Tonemaped_Gamma_HDR.cube - HDR correction LUT (Rec.2020 primaries)
* PG39WCDM_LUT-s32.png - 32x 3D correction LUT - PNG
* PG39WCDM_LUT-s32.dds - 32x 3D correction LUT - DDS (bc7, no mips)

The PNG LUT can be used with Reshades 'LUT' basic LUT shader if you wanted to correct only a single SDR game.
Either Apply the LUT at system level, or in an application. If you apply it twice you will over-correct.
The .cube files are compatible with professional color pipelines and solutions. If you know how to use them, have fun. If not, you're not missing out on anything crucial. They serve niche needs.