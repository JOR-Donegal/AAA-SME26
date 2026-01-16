# Introduction

!!! abstract "AAA for an SME"

On any Greenfield site my starting point would be to build the core services. In almost every case these will be built around Active Directory. Beware Microsoft is trying to push customers in the direction of their public cloud, Azure. Note that anything I do using Microsoft technology, I could replicate, license free, using Linux.

I am trying to achieve authentication, authorization and accounting (AAA). By far and ahead the most common solution for this in Ireland will be Microsoft's Active Directory (AD).

In these exercises I am going work my way through from my first questions to my customer, to a good working prototype which can be deployed in a private cloud environment. Note that I will take a similar approach in Public Cloud. I'm going to start with a table of data that I need my client to populate with the minimal information I need to set up the systems.

| Requirement      | Data                |
| ---------------- | ------------------- |
| Internet Domain  | electric-petrol.ie  |
| Local site       | Head Office         |
| Address range    | 10.0.0.0/16         |

I am going to need some other details, in particular assignment of VLANs, hosts and servers. 

## Customer's requirements
This build is for a single site.  

The customer does not have experienced full-time staff so they are very nervous with regard to system security. To assure the customer, we have agreed that there will be no internet or off-site connectivity whatsoever. We also need to build the site with resilience and high availability by default. We are going to have to have some sort of local solution for backup and recovery. And whatever solution we design will need to be as simple as possible.

## Proposed solution
To meet these requirements, we will build a private cloud solution. There will be two hosts servers, each running a hypervisor. Each host will have Whatever servers are required for the customer's solution. And each of these hosts will maintain a copy of the service on the other host. The host servers themselves should be based on high availability hardware.