![alt text](data-center-shot.jpg)

# Analysis of traffic workloads in Facebook datacentres


## Background
Datacenters have become prevalent in offering managed compute and storage to businesses. The backbone of a datacenter is the network. Much of its configuration and management, including traffic engineering and provisioning, depends on the service workload. Naturally, workloads vary over datacenters depending on customer base, operator offering and geographical scope.

Most publicly available discussion on datacenter workloads stem from Microsoft datacenters but the authors of [this paper](fb-sigcomm15.pdf) show there are discrepancies when compared to a Facebook datacenter. Nevertheless, the authors also state, there are similarities. There is this value in **understanding and statistically describing available datacenter workloads** for better network management and increased customer satisfaction. 

Understanding the properties of a datacenter workload strongly affects the efficient operation and management of the network. At the same time workloads experience rapid change so having fresh data is critical. Facebook [makes available](fb-sigcomm15.pdf) the latest study on datacenter workloads. It is also the first one to report on production traffic in a datacenter network connecting hundreds of thousands of 10-Gbps nodes.

## Problem Description

<img src="https://code.fb.com/wp-content/uploads/2014/11/GNbKowDUKNqKwcECAEXsXkcAAAAAbj0JAAAB.jpg" width="350px" align="right" />

We wish to study the statistical characteristics of a publicly accessible workload from Facebook datacenters and subsequently develop a model which faithfully capture those characteristics. Subsequently, this will allow us to create a generator of synthetic workloads which resemble those observed by Facebook. Such traffic generator benefits variety of studies on network design and traffic engineering including load balancing, QoS provisioning, etc.

This project will initially centre around exploratory data analysis. Examples of questions you may wish to be able to answer through the EDA process could include the following:

- How do traffic characteristics vary over time?
- What patterns are present in the transmission of data? Are there particular source-destination pairs which represent a large quantity of traffic?
- How do different workload types (i.e. Database, Web, Hadoop) exhibit different traffic characteristics?
- What patterns are present with respect to rack- and pod-locality?
- What patterns are present with respect to inter-cluster and inter-datacenter communication?
- Is there any evidence within the data to suggest the level of co-location of applications on servers?

## References
> Roy, Arjun, Hongyi Zeng, Jasmeet Bagga, George Porter, and Alex C. Snoeren. "Inside the social network's (datacenter) network." In *ACM SIGCOMM Computer Communication Review*, vol. 45, no. 4, pp. 123-137. ACM, 2015. [[paper]](fb-sigcomm15.pdf)


## Dataset
The dataset can be accessed at the following OneDrive location:

https://newcastle-my.sharepoint.com/:f:/g/personal/nmf47_newcastle_ac_uk/Eo3L7lp1rV5GiMErL47YzQcBTaVjU7ssVsVVzMYU38afcg?e=lMWyWy

<!--The dataset can be accessed by email Matt at <matthew.forshaw@ncl.ac.uk> who will give you access to a OneDrive location which contains the dataset.-->
<!--Thank you for your interest in FB datacenter data sharing program. Please download the data from https://www.facebook.com/network-analytics. Here are some FAQs.-->

<!--1. Where is the data from?-->

The data consists raw fbflow samples from 3 of Facebook's production clusters. 
- **Cluster-A** is for Database, 
- **Cluster-B** is for Web servers, - if you would like access to Cluster B, you can obtain these directly from https://www.facebook.com/network-analytics. To access this link you will need to join and be approved by the group https://www.facebook.com/groups/1144031739005495/.
- **Cluster-C** is used as Hadoop servers.

All three clusters are from Facebook's [Altoona Data Center](https://www.facebook.com/AltoonaDataCenter/?ref=gs&__tn__=%2CdK-R-R&eid=ARBSAPg5oE2dLOuRqp5AGll5KLifuBqtJ8Dv1SyMzCWWn2dx5O-sA10Tecj9MzlnFVJMDxV2-1zFh2ij&fref=gs&dti=1144031739005495&hc_location=group). You can find more details about the datacenter topology in https://code.facebook.com/posts/360346274145943/.

The entire Facebook dataset is extremely large (>50GB while compressed), but this is primarily due to **Cluster-B**. When handling this data you are best advised to select a subset, either by focusing on an individual cluster, or loading in just a subset of the archives for your analysis. When you study [Big Data Analytics](https://www.ncl.ac.uk/postgraduate/modules/CSC8101) with Paolo Missier in the New Year, you will see approaches to perform analysis on datasets of this magnitude.

The sampling rate is 1:30,000, uniform, per-packet sampling.

<!--2. How to download?Each row of the scroll list is a URL of a data chunk in bz2 format. And it can be downloaded by wget or other downloaders. To unzip it, simply execute "bunzip2 <the filename to unzip>.bz2".-->

<!--3. What's the file format?-->

After decompressing each archive (or a subset of these), the text file is in tsv format with each row as a raw sample in the following format:

- timestamp
- packet length (1)
- anonymized(2) source (src) IP
- anonymized(2) destination (dst) IP
- anonymized source (src) L4 Port
- anonymized destination (dst) L4 Port
- IP protocol
- anonymized source (src) hostprefix (3)
- anonymized destination (dst) hostprefix (3)
- anonymized source (src) Rack
- anonymized destination (dst) Rack
- anonymized source (src) Pod
- anonymized destination (dst) Pod
- intercluster
- interdatacenter

Note:

1. FB uses TCP segmentation offload, hence packet length can be larger than 64KB since we are sampling outgoing packet in kernel.
1. We hash the content and get a subset of the hash value. So it is not guaranteed to be a 1:1 mapping.
1. Host prefix is the prefix of hostname. For example, a machine "web102.prn1.facebook.com" has hostprefix "web". It's a very rough classification of machine types. But notice that multiple programs can run on the same machine.


---

## Contact

If you have any questions regarding the data please include the following addresses in the **To:** line and clearly state **CSC8634** in the subject line so I prioritise picking up your query.

Matthew Forshaw
<matthew.forshaw@newcastle.ac.uk>
