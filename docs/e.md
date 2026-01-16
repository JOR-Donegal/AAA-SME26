# Configure Active Directory
Microsoft is moving on from Server Manager, but for now, do these exercises in Server Manager.

Under Tools, open the following plugins and configure

## Active Directory Sites and Services
1. Change the Default-First-Site-Name to an appropriate site name. For me, this is __head office__.
2. Add the subnets appropriate for your site.

<figure>
<img src = "https://jor-donegal.github.io/AAA-SME26/images/fig3.jpg">
<figcaption>Fig 3. Configuring sites and subnets.</figcaption>
</figure>

If HeadOffice had more than one physical site, we would document them with unique names here.

For this simple project, we will leave inter-site transports as they are. In a complex project with multiple sites, we may define topologies here. Way too complicated for this exercise!

## Active Directory Doamins and Trusts
This is a single domain and single site project. This is the simplest topology we can have but also the most common.

Note that we can upgrade the AD database from here (raising functional level), something we rarly have to do.

There is one [FSMO role](https://johnoraw.gitbook.io/active-directory-1/active-directory/fsmo) visible from here, the operations/naming master.

<figure>
<img src = "https://jor-donegal.github.io/AAA-SME26/images/fig3.jpg">
<figcaption>Fig 4. A FSMO role.</figcaption>
</figure>

## Active Directory Users and Computers
I do not want to go into _Group Policies_ in this module, but it is probably the most important single reason for the success of Active Directory. It would cost us at least a week of theory and practice and it's well off the theme of this module. It's based on the idea that we can build a hierarchical structure in Active Directory and apply policies at every level. A tool this powerful can be Incredibly destructive! For this reason we have some simple rules we apply.

In the plugin we see a top level to the hierarchy, ho.electric-petrol.ie. We never want to set policies at this level, as they apply to the entire domain and I have seen technical staff locking out entire organizations by making an error at this level.

Looking at the folders under this level, we see a difference in the symbol for domain controllers. This is a symbol for an organizational unit (OU). There is nothing special about a folder, but an OU allows policies to be applied to every object under that OU.

Best practice is to create a top level OU at the highest level we might want to impose policies and then to build a structure underneath that; I called mine __Head Office__. This is not based on tidying up objects, thast is what folders are for! This structure is based on the levels at which you might want to apply distinct policies.

<figure>
<img src = "https://jor-donegal.github.io/AAA-SME26/images/fig5.jpg">
<figcaption>Fig 5. Organisational units.</figcaption>
</figure>
 