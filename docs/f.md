# Configure DC2
I created a Windows 2025 Core golden image earlier.
 I clone that as __dc2.ho.electric-petrol.ie__ and I'm going to configure it the easy way for now.
Microsoft said they were going to deprecate the main tool we are going to use, but no sign of that yet.

When you login, __sconfig__ should auto run.

<figure>
<img src = "https://jor-donegal.github.io/AAA-SME26/images/fig6.jpg">
<figcaption>Fig 6. Sconfig.</figcaption>
</figure>

1. Change the computer name to dc2
2. Change the IP address to 10.0.6.32/24. If everything has worked correctly, it will already have a DHCP address from DC1.
3. The DNS must point at DC1 (why?), 10.0.6.32.
4. Now do a restart.
5. Exit to the command line and ping 10.0.6.31, that will check connectivity.
6. Ping dc1.ho.electric-petrol.ie, name resolution must work before we continue.
7. Now run __sconfig__ again.
8. Join the domain _ho.electric-petrol.ie_
9. Reboot

I said I was going to do this the easy way. Now that the server DC2 is in the domain, I can do all control and configuration from DC1, which has a graphical user interface.

In Server Manager I now add a server.

<figure>
<img src = "https://jor-donegal.github.io/AAA-SME26/images/fig7.jpg">
<figcaption>Fig 7. Server Manager.</figcaption>
</figure>

I type DC2 and Find Now.

<figure>
<img src = "https://jor-donegal.github.io/AAA-SME26/images/fig8.jpg">
<figcaption>Fig 8. Adding a server.</figcaption>
</figure>

I can now control DC2 as if it were the local server.

<figure>
<img src = "https://jor-donegal.github.io/AAA-SME26/images/fig9.jpg">
<figcaption>Fig 9. DC2 listed.</figcaption>
</figure>

1. I right click DC2 and Add Roles and Features.
2. I add Doamins Services, DHCP and DNS and I allow the server to restart if needed.
3. I click Install

After installation is complete, I need to promote DC2 to being a domain controller. By default, the wizard will try to add to the existing doamin and that is fine.

1. Provide credentials when requested, the administrator account of the domain.
2. Use the default site name HeadOffice and provide a DSRM password.
3. Do not worry about the DNS delegation error!
4. Use defaults for the rest of the settings, but save the script when you get the option, this is part of your documentation for the client.

On the final screen, you will again get a delegation error, ignore this and click __Install__.

After a few minutes you have a second DC>

### Exercise
1. DNS is replicated and integrated into DNS, confirm DC2 is acting correctly as a DNS
2. DHCP is unconfigured for now on DC2.


