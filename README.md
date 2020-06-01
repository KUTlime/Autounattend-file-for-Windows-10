# Autounattend file for Windows 10
> A repository to track my autounattend files for Windows 10 installations to inspire other how to save some time and stress.

# About
Every 6 months, Microsoft release a new *major* version of Windows 10. The upgrade feature is sensitive to Windows customization, e.g. hosts file. If you inject some counter advertise or tracker entries, you will most likely break auto upgrade.

A clean installation is only other option which you have. If you know PowerShell, you can replace about 96 % of repetitive work after installation. I don't know PowerShell, feel free to contact me and we can discuss this option but for now, lets star with Windows 10 auto or semi auto installation.

Windows 10 supports an unattended installation for years. This repo contains some examples of `autounattend.xml` files for basic scenarios like 1 physical drive + local account. Normally, you would have install Windows System Image Manager, go through the catalogue, choose from various options even for simple scenarios like the one above. This is very unproductive way how to do it.

The fact is that the Answer file settings doesn't change much with new Windows 10 releases and you recycle your existing files. For simple scenarios, you could easily generate the `autounattend.xml` file with **Windows Answer File Generator**, see [this](https://www.windowsafg.com/index.html) link.

# How to use autounattend file?
## 1. Physical drive
Download the `autounattend.xml` file, customized it (*e.g. partitioning or user name/password*), put it the root of your physical drive and your are good to go.

## 2. ISO file
This option is a little bit complicated but necessary if you want test your autounattend files before production use, e.g. in Hyper-V environment.
1. Download your Windows ISO image.
2. Download and install a software to edit ISO images. My recommendation is [PowerISO](https://www.poweriso.com/download.php). There are even some portable edition of this tool.
3. Edit the ISO file as it would be a physical drive. Put your `autounattend.xml` file into the root of ISO file.
4. Save the modified ISO file as a new file preferably.
5. Test it! 
6. Put your new `autounattend.xml` file to your deployment strategy.

# Product key and how to use it
If your Windows Image Installation file contains more than one Windows version installation, you could specify a product key for a version that you want to install.

Microsoft provides [generic keys](https://docs.microsoft.com/en-us/windows-server/get-started/kmsclientkeys) which can be used to determine a version that would be installed.

If you specify:

```xml
<ProductKey>
    <Key></Key>
    <WillShowUI>Never</WillShowUI>
</ProductKey>
```

in your `autounattend.xml`, you will be asked for a selection of desired version.

# How to find a meaning of any XML section or key?
Microsoft provides documentation to unattended installation at its Docs portal, see this [link](https://docs.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/).

On the left side, there is a component menu where you can copy/paste any XML element from the `autounattend.xml` file and see what it does.

# How about Microsoft Accounts?
The unattended installation feature is designed to Enterprise segment. Microsoft accounts are not supported because of this. Only possible solution is to install local account and convert it into Microsoft account.

In this scenario, you have to *emulate* your user folder structure. If your Microsoft Account create a folder e.g. `john` in `C:\Users\`, so the environmental variable `USERPROFILE` points to `C:\Users\john`, in order to save yourself **a lot** of troubles, you have to create your local account with `username` equal to `john`.

```xml
<LocalAccounts>
    <LocalAccount wcm:action="add">
        <DisplayName>John Doe</DisplayName>
        <Group>Administrators</Group>
        <Name>john</Name>
    </LocalAccount>
</LocalAccounts>
```

Why all this? There are software that doesn't correctly use environmental variables and their settings get synchronize across your devices. You can easily end up in the situation when one software works perfectly fine on one device but has some strange path-related errors on another device.

A funny thing is that when I've tried this in Hyper-V (*multiple times*), I wasn't able to log in after conversion of my local account. Combo boxes with user name and password were missing at the login page. It was blank. 

Does make any sense? IMHO it doesn't. It is just pure 💩.

### Conclusion
If you want to save yourself a lot of troubles and have Microsoft Account out of the box, **you are stuck with attended installation**.
