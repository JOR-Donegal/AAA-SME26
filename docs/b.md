# Hosts
I am going to keep this really simple. 
I will need two high availability servers and I will locate tham as far apart as possible, in the most secure/safe locations I can find on site. I plan for a hardware lifetime of five years.

In my notes below, __big B__ means Bytes, __little b__ means bits!

## Processor
Nothing about this project seems to demand high processing power,  but it will use virtualization. I would probably order a two processor system where each processor has at least eight cores.

## Primary memory
Each server will run a standard hypervisor; I'm going to allocate 64 gigabytes for that. To be generous, I'm going to allocate 64 gigabytes for each server type I will run. If I assume that each host will have a domain controller, a file server, a database server, and two application servers, I should estimate a minimum of 320GB of DRAM. I will specify 512GB. A modern server will probably have a capacity of >1TB of DRAM. I am going to order the server with the largest DRAM sticks available. This will initially be more expensive, but it will allow me to upgrade over the servers lifetime, without throwing away expensive memory. I will be ordering registered, bufferred DRAM, I cover why in the theory notes on memory.

## Boot partition
Any modern server will have a boot-optimized storage system specifically for the hypervisor operating system. I'm normally going to specify two small NVMe drives for this.

## Main Storage
There are three main considerations in any storage design. 

- What capacity will be required over the lifetime of the system?
- What is the availability requirement?
- What is the performance requirement?

We can estimate 50GB per VM operating system, but we will need information from the customer relating to the applications to estimate file and database storage requirement. For now, assume the customer has estimated no more than 1TB/year. Our estimate is now c. 6TB, we should design for 10TB.

We will look at availability calculations later in this module. For now, let us specify a _RAID controller_ and plan to use RAID6 only.
Similarly, we will look at performance calculations later in the module. In this example, the required throughput would seem to fairly trivial, we will assume that the disk/controller combination will be sufficient. This would not be unusual on a small project. 

### Beware!!
There are issues using RAID controllers with Microsoft's _Clustered Shared Storage_ technology on clusters. For this project, I am assumming we wil __not__ use clusters. 

## Networking
Almost every server has two 1 Gb/s ports on its motherboard. We will use these for host management. These will normally connect to two separate _Top of Rack (ToR) switches_.

There will be an _out of band management_ device with a 1Gb/s connection. In a data center, there may be a dedicated management switch for this. On a small project like this, we will just connect it to one of the ToR switches.

The host's purpose is to present servers and services for consumption by clients. We will normally provide two high speed connections, one to each ToR switch, and then use these for trunks or tagged traffic. These days, high speed means 10 to 100 Gb/s.

The most demanding connections will be for back-end storage. We will look at this in more detail later in the module. There would be two high speed connections to a dedicated _Back End (BE) switch_. In this project, the customer has not specified any storage infrastructure, but we are going have to consider what that might be, in particular to allow backups. On a larger project, this might be quite complex and require us to use a specialized _Host Bus Adapter_ (HBA). We will deal with _Storage Area Networks_ later in this module.

## Other characteristics
Any server we procure will have dual power supplies, hot pluggable.

We will rack mount the server and ensure that there are two UPS devices close by.





