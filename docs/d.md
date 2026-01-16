# Configure DC1
I am using VMWare Worktation, adapt my instructions to whatever environment you are working in.
I am going to use 

1. First, I create a full clone of my desktop image. I am the headoffice site, I will abbreviate that to __HO__. 
My first domain controller will be _dc1.ho.electric-petrol.ie_
2. I create a host-only network to match VLAN6.
3. I power up __dc1__.
4. I rename this VM, as it will still have the generic name of the gold image.

```
# Name the first DC 
Rename-Computer -NewName dc1
```

5. I give it a static IP address of 10.0.6.31/24. There is no gatway, but I'll use 10.0.6.20. First I find the interface ID

```
# Check existing IP addresses & interface ID
Get-NetIPAddress
```

For me, if was InterfaceIndex 4

<figure>
<img src = "https://jor-donegal.github.io/AAA-SME26/images/fig2.jpg">
<figcaption>Fig 2. Get-NetIPAddress.</figcaption>
</figure>

Now I set the correct IPv4 settings for __dc1__.

```
New-NetIPAddress -InterfaceIndex 4 -IPAddress 10.0.6.31 -PrefixLength 24 -DefaultGateway 10.0.6.20
```
Finally, I reboot to make these settings current.
````
# Now reboot
Restart-Computer
````

## Install features for a new forest
Now __dc1__ is ready for me to install active directory. 
````
# Set the forest name
$FOREST = 'ho.electric-petrol.ie'
# Install the required features
Install-WindowsFeature -name AD-Domain-Services -IncludeManagementTools
# Create the root domain, all defaults except the domain name
Install-ADDSForest -DomainName $FOREST
# Reboot
Shutdown /r /t 0
````
### 

The system should now reboot. Using Server Manager, look through the standard tools for Active Directory to verify.

## Verify and configure DHCP
We need to install DHCP and authorize it in AD. Check and verify as you go along, using the Server Manager plugin for DHCP.
```
# Install the feature
Install-WindowsFeature DHCP -IncludeManagementTools
# Authorize in AD
Add-DhcpServerInDC -DnsName dc1.ho.electric-petrol.ie -IPAddress 10.0.6.31
```
In DHCP, we normally set options for the entire server first, check the __Server Options__ tab in the management tool.
```
Set-DhcpServerv4OptionValue -DnsDomain ho.electric-petrol.ie
Set-DhcpServerv4OptionValue -DnsServer 10.0.6.31
```
Then we can set up individual scopes and any options that are particular to that scope.
```
Add-DhcpServerv4Scope -name "Clients" -StartRange 10.0.5.100 -EndRange 10.0.5.199 -SubnetMask 255.255.255.0 -State Active
Add-DhcpServerv4ExclusionRange -ScopeID 10.0.5.0 -StartRange 10.0.5.0 -EndRange 10.0.5.99
```

## Verify and configure DNS
Verify that DNS was installed by default. Check and verify as you go along, using the Server Manager plugin for DNS.
```
# Make sure DNS was installed
Get-WindowsFeature | where {($_.name -like "DNS")}
```
Forward lookup zones are created by default, but we need to manually create the reverse lookup zones.

```
Add-DnsServerPrimaryZone -NetworkId 10.0.2.0/24 -ReplicationScope Domain
Add-DnsServerPrimaryZone -NetworkId 10.0.5.0/24 -ReplicationScope Domain
Add-DnsServerPrimaryZone -NetworkId 10.0.6.0/24 -ReplicationScope Domain
```
## Functional test
From the command prompt, do an __nslookup__ against this server, for both forward and reverse lookup zones.


## Finally
Loads done!

As you go on, you will find that there are many other things you want to set. As you identify these, figure out the Power Shell for doing these tasks.


