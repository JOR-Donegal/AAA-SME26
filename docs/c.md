# Servers (VMs)

I can build the virtual machine images for this project on any compatible host in most cases I can sit at home and build them on my laptop. For simplicity I'm going to assume that my hosts our Windows Server 2025 running hyper V. I'm going to assume that my VM's will all be Windows Server also.

I will not build the hosts until I get to the customer site, and I have the actual customer hardware. For now, I am going to build the virtual machines. Initially, I'll build the Goldie bridges of a desktop and core instance of Windows Server. I will run through my checklist of things to do and seek guidance from whoever is advising the client on cybersecurity. Ideally, the custom rule of precise guidelines for me, but this is normally not the case on an SME site. I might use this as an excuse to get the customer to write some policy documents and Standard Operating Procedures (SOPs). 
Once the VMS are created, I will patch them up to date.

I now build and test 
- w25-desktop
- w25-core

I am going to assumme you can build Windows GUI instances, and that build __w25-desktop__ using the Windows Server 2025 Standard ISO. Make sure it is set to the correct time zone.

### Do updates

Open PowerShell ISE as Administrator and run

```
Install-Module -Name PSWindowsUpdate
```
You may be asked questions about installing other software. This is safe, go ahead.


To see what updates are available, 
```
Get-WindowsUpdate
```

I can install all of these using
```
Get-WindowsUpdate -AcceptAll -Install -AutoReboot
```
Shut down the VM and document it.

## Build a Core instance
Go through the normal installation dialog but select __Windows Server 2025 Standard __

<figure>
<img src = "https://jor-donegal.github.io/AAA-SME/images/fig1.jpg">
<figcaption>Fig 1. Install options.</figcaption>
</figure>

1. On reboot, you will be required to set an administrator password.
2. You may then be asked some stndard questions.
3. SConfig will then load, which will allow you to do the initial configuration. 
4. Change the time zone and make sure the time is correct. 
5. Do updates (all quality updates) and reboot.
6. Shut down the VM and document it.


