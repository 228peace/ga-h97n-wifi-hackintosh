# GA-H97N-WIFI hackintosh

![Iris Pro 6200 on Gigabyte H97N-WIFI](https://raw.githubusercontent.com/vulgo/ga-h97n-wifi-hackintosh/main/Images/ga-h97n-wifi-broadwell.jpg)

OpenCore configuration for running macOS on the Gigabyte H97N-WIFI motherboard.

## Firmware Settings

#### BIOS Features

| Field              | Value             |
|:-------------------|------------------:|
| Fast Boot          | Disabled          |
| VT-d               | Enabled           |
| Windows 8 Features | Windows 8 WHQL    |
| CSM Support        | Never             |
| Secure Boot        | Disabled          |

#### Peripherals

| Field                                        | Value    |
|:---------------------------------------------|---------:|
| XHCI Mode                                    | Enabled  |
| Intel Processor Graphics                     | Enabled  |
| Intel Processor Graphics Memory Allocation   | 64M      |
| DVMT Total Memory Size                       | MAX      |
| Legacy USB Support                           | Disabled |
| XHCI Handoff                                 | Enabled  |
| Super IO Configuration &#8594; Serial Port A | Disabled |

Source: [https://dortania.github.io/OpenCore-Install-Guide/config.plist/haswell.html#intel-bios-settings](https://dortania.github.io/OpenCore-Install-Guide/config.plist/haswell.html#intel-bios-settings)

## OpenCore Sanity Checker

[https://opencore.slowgeek.com/](https://opencore.slowgeek.com/)

## Graphics

Edit the ```DeviceProperties``` section of your config.plist so that the value for ```AAPL,ig-platform-id``` matches your configuration.

```xml
...
<key>DeviceProperties</key>
<dict>
    <key>Add</key>
    <dict>
        ...
        <key>PciRoot(0x0)/Pci(0x2,0x0)</key>
        <dict>
            <key>AAPL,ig-platform-id</key>
            <data>BAASBA==</data>
        </dict>
        ...
    </dict>
</dict>
...
```

| AAPL,ig-platform-id | Base64   | IGPU Configuration                    |
|:--------------------|:---------|--------------------------------------:|
| 0300220D            | AwAiDQ== | Attached display                      |
| 04001204            | BAASBA== | Headless (discrete GPU)               |
| 07002216            | BwAiFg== | Broadwell (attached display/headless) |

Source: [https://dortania.github.io/OpenCore-Install-Guide/config.plist/haswell.html#deviceproperties](https://dortania.github.io/OpenCore-Install-Guide/config.plist/haswell.html#deviceproperties)

## SystemProductName

Edit the ```PlatformInfo``` section of your config.plist so that the value for ```SystemProductName``` matches your hardware.

```xml
...
<key>PlatformInfo</key>
<dict>
    ...
    <key>Generic</key>
    <dict>
        ...
        <key>SystemProductName</key>
        <string>iMac15,1</string>
        ...
    </dict>
    ...
</dict>
...
````

| CPU       | SystemProductName |
|:----------|------------------:|
| Haswell   | iMac15,1          |
| Broadwell | iMac16,2          |

Source: [https://dortania.github.io/OpenCore-Install-Guide/config.plist/haswell.html#platforminfo](https://dortania.github.io/OpenCore-Install-Guide/config.plist/haswell.html#platforminfo)

## SMBIOS

Edit the ```PlatformInfo``` section of your config.plist so that the ```MLB```, ```ROM```, ```SystemSerialNumber``` and ```SystemUUID``` values are unique to your machine.

```xml
...
<key>PlatformInfo</key>
<dict>
    ...
    <key>Generic</key>
    <dict>
        ...
        <key>MLB</key>
        <string>M0000000000000001</string>
        ...
        <key>ROM</key>
        <data>ESIzRFVm</data>
        ...
        <key>SystemSerialNumber</key>
        <string>W00000000001</string>
        <key>SystemUUID</key>
        <string>00000000-0000-0000-0000-000000000000</string>
    </dict>
    ...
</dict>
...
````

| PlatformInfo &#8594; Generic | Source                    |
|:-----------------------------|--------------------------:|
| MLB                          | \**Board Serial*          |
| ROM                          | 6 bytes, e.g. MAC address |
| SystemSerialNumber           | \**Serial*                |
| SystemUUID                   | \**SmUUID*                |

\* *GenSMBIOS output, iMac15,1 (Haswell) or iMac16,2 (Broadwell)*

Source: [https://dortania.github.io/OpenCore-Install-Guide/config.plist/haswell.html#platforminfo](https://dortania.github.io/OpenCore-Install-Guide/config.plist/haswell.html#platforminfo)

GenSMBIOS: [https://github.com/corpnewt/GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)

## USB

Edit the USBPortInjector.kext ```Info.plist```. See [README-USBPortInjector.kext.md](https://github.com/vulgo/ga-h97n-wifi-hackintosh/blob/main/README-USBPortInjector.kext.md)

## First boot

At the picker, **press space**, choose **Reset NVRAM**.

## Post-Install

[https://dortania.github.io/OpenCore-Post-Install/](https://dortania.github.io/OpenCore-Post-Install/)
