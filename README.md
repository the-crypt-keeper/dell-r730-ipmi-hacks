# Dell R730 GPU and Fan noise hacks

To get 2xP40 behaving in an R730 is a bit of a puzzle despite this card actually being on the supported devices list. 

To both not overheat and be as quiet as possible, you need to:

* Set the power management for 'Performance/Watt' in both the BIOS and iDRAC
* Set the minimum fan speed to 20% - the default is 0% but this will make the P40s overheat when idle
* Disable 3P Fan Acceleration

## 3P Fan Acceleration

The R730 has a terrible feature where if the BIOS detects a PCI-ID that it doesn't recognize it will run the fans at the full 100% 15k RPM and you will go deaf.

Use the `enable_3p` and `disable_3p` scripts to control this feature (the desired state is disabled). To see the current state use the `get_3p` script and look at the 3rd last byte - if it's 0x01 this feature is disabled, if it's 0x00 it's enabled.

## Manual Fan Controls

If you push the P40s past ~180W then 20% fans won't be able to keep up, but the system doesn't know it needs to ramp the fans.  Use the `fan_manual_on` script to temporarily disable the automatic fan controls and ramp to 40%.  When you're done, run `fan_auto` to go back to BIOS controls which handle correctly ramping fans if the CPUs are utilized.