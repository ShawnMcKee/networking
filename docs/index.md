OSG Networking Area
===================

<<<<<<< HEAD
*Welcome to OSG Networking !* This is an entry point for those interested in Networking in OSG/WLCG or for those OSG/WLCG users experiencing network problems. It provides an overview of the networking goals, plans and various activities and subtopics underway regarding networking in the *Open Science Grid (OSG)* and *World-wide LHC Computing Grid (WLCG)*, operated as a joint project. This area started in June 2012 with initial focus on the network monitoring as monitoring is critical to provide needed visibility into existing networks and site connectivity. OSG is working to provide needed networking information and tools for users, sites and experiments/VOs.

This documentation is divided into several sub-sections, each covering a specific area of activities. 

Network Monitoring in WLCG and OSG (perfSONAR)
----------------------------------------------

WLCG and OSG jointly operate a network of `perfSONAR` agents deployed world-wide, which provides an open platform that can be used to baseline network performance and debug any potential issues. The following subsections provide details on the motivation, deployment and operations of the perfSONARs in WLCG/OSG: 

- [Motivation](perfsonar-in-osg.md) - overview, core concepts, motivation
- [Deployment Guide](perfsonar/deployment-models.md) - deployment models and options, hardware requirements
- [Installation and Administration Guide](perfsonar/installation.md) - installation, configuration and maintanance 
- [Frequently Asked Questions](perfsonar/faq.md)

Network Troubleshooting
-----------------------
Users with network issues should check the [troubleshooting link](network-troubleshooting.md) below for initial guidance on how best to get their issue resolved. In addition, you can refer to the [ESNet network performance guide](https://fasterdata.es.net/performance-testing/troubleshooting/network-troubleshooting-quick-reference-guide/) for a detailed instructions on how to identify and isolate network performance issues using perfSONAR.

Network Services 
----------------

OSG operates an advance platform to collect, store, publish and analyse the network monitoring data it gathers from perfSONAR and other locations. All measurements are collected and available via streaming or through APIs. The following services are available:

- [perfSONAR infrastructure monitoring](perfsonar/psetf.md) - monitors state of perfSONAR network and reports on availability of core services
#- *OSG Network Datastore* - central datastore holding all the network measurements and providing an API to expose them via JSON. Datastore is based on [ESMOND](http://software.es.net/esmond/), which supports the following [API](http://software.es.net/esmond/perfsonar_client_rest.html) and runs at this [endpoint](http://psds.opensciencegrid.org/esmond/perfsonar/archive/?format=json).
- *OSG Network Stream* - access to network measurements in near realtime is provided by the GOC RabbitMQ and CERN ActiveMQ messaging brokers.
- *OSG Mesh Configuration Interface (MCA)* - centralized configuration of the tests performed by the OSG/WLCG perfSONAR infrastructure (see [MCA](http://docs.perfsonar.net/mca.html) for details). In case you'd like to start/manage particular mesh, please contact our support channels to get access.
- [*OSG Dashboards*](http://psmad.opensciencegrid.org/maddash-webui/index.cgi) - set of dashboards showing an overview of the network state as seen by the perfSONAR infrastructure 
- [*WLCG Dashboards*](http://monit-grafana-open.cern.ch/dashboard/db/home?orgId=16) - set of dashboards showing WLCG and OSG network performance by combining multiple sources of data including perfSONAR, FTS, ESNet/LHCOPN traffic, etc. 

NOTE: As of May 2018, OSG is no longer supporting a central perfSONAR measurement archive based upon ESmond.  Instead all data is accessible from one of our two Elasticsearch instances at University of Chigago or Nebraska.

Network Analytics
-----------------
University of Chicago has setup an [**analytics platform**](<https://twiki.cern.ch/twiki/bin/view/AtlasComputing/ATLASAnalytics>) using `ElasticSearch` and `Kibana4` as well as `Jupyter` that can be used to access and analyse all the existing network measurements.

Support and Feedback
--------------------
If you suspect a network problem and wish to follow up on it, please open a ticket with the appropriate support unit: For `OSG` sites please open a ticket with [GOC](http://ticket.opensciencegrid.org/submit); For `WLCG` sites please open a [GGUS](https://ggus.eu/) ticket to `WLCG Network Throughput` support unit. If you'd like to get help in setting up or debugging perfSONAR instance please open a ticket with [GOC](http://ticket.opensciencegrid.org/submit) or via [GGUS](https://ggus.eu/) to WLCG perfSONAR support. For any other requests please contact [GOC](http://ticket.opensciencegrid.org/submit).


References
----------
- ESNet network performance tuning and debugging <https://fasterdata.es.net/>
- [perfSONAR](http://docs.perfsonar.net/) toolkit is part of the [perfSONAR](http://www.perfsonar.net/) project. 
- **OSG/WLCG mesh configuration interface** is available at http://psconfig.opensciencegrid.org 
#- Information on **ESmond** is available at <http://software.es.net/esmond/>
#- Information on querying the perfSONAR data from ESmond is at <http://software.es.net/esmond/perfsonar_client_rest.html>
#- Access to a JSON view of the **OSG network datastore** is available at <http://psds.opensciencegrid.org/esmond/perfsonar/archive/?format=json>
- **OSG dashboard instance** <http://psmad.opensciencegrid.org/maddash-webui/index.cgi>
- **OSG perfSONAR infrastructure monitoring** <https://psetf.opensciencegrid.org/etf/check_mk/>
- **OSG Analytics platform** <http://atlas-kibana.mwt2.org:5601/app/kibana#/dashboard/Default?_g=()>
- **WLCG dashboards** http://monit-grafana-open.cern.ch/dashboard/db/home?orgId=16
- **PuNDIT** (an OSG Satellite project) focusing on analyzing perfSONAR data to alert on problems: <http://pundit.gatech.edu/>
- **MadAlert** which analyzes **MaDDash** meshes to identify network problems: <http://madalert.aglt2.org/madalert/>

=======
Welcome to the home page of the OSG Networking Team documentation area! This is currently a work in progress as we migrate our TWiki documentation to GitHub. If you are looking for our full documentation, please visit the [TWiki](https://twiki.opensciencegrid.org/bin/view/Documentation/NetworkingInOSG).

<span class="twiki-macro ICON">hand</span> \*Welcome to OSG Networking\* This is the entry point for those interested in Networking in OSG or for those OSG users experiencing network problems. It provides an overview of the networking goals, plans and various activities and subtopics underway regarding networking in the Open Science Grid. This area for OSG started in June 2012 and initially focused on network monitoring. Monitoring is critical to provide needed visibility into existing networks and site connectivity. OSG is working to provide needed networking information and tools for OSG users, sites and VOs.

Users with network issues should check the [troubleshooting link](network-troubleshooting) below for guidance on how best to get their issue resolved.

perfSONAR Deployment Details for WLCG and OSG
---------------------------------------------

We have created a set of pages to guide the deployment of perfSONAR for use by WLCG and OSG. Please refer to DeployperfSONAR to find details about installing updating and user perfSONAR in these communities.

OSG Networking Components
-------------------------

The plans for OSG networking include activity in these areas related to network monitoring:

-   The perfSONAR toolkit: development, packaging, configuring and supporting the toolkit in the context of OSG but potentially applicable to a diverse set of users and VOs.
-   The OSG Network Service: targeting the development, packaging, deployment and maintenance of a network service which collects, aggregates, displays and provides network related metrics from the perfSONAR toolkit deployments at OSG and WLCG sites.
-   [Network problem solving documentation](https://twiki.opensciencegrid.org/bin/view/Documentation/NetworkingTroubleShooting): Updating and augmenting OSG documentation related to network problem troubleshooting using OSG distributed tools, software and services.

Other Useful Networking References
----------------------------------

OSG supports a number of services for networking including a **central ESmond datastore** which hosts all the OSG/WLCG perfSONAR data.

-   Information on ESmond is available at <http://software.es.net/esmond/>
-   Information on querying the perfSONAR data from ESmond is at <http://software.es.net/esmond/perfsonar_client_rest.html>
#-   Access to a JSON view of the OSG network datastore is available at <http://psds.opensciencegrid.org/esmond/perfsonar/archive/?format=json>

We can monitor the OSG network metrics using **MaDDash** (ESnet's Monitoring and Diagnostic Dashboard)

-   OSG **MaDDash** instance <http://psmad.opensciencegrid.org/maddash-webui/index.cgi>

The **basic service checking** to track and monitor all our perfSONAR services uses `OMD/Check_mk`. NOTE: You need an x.509 credential in your browser to view this

-   `OSG OMD/Check_MK` service monitoring <https://psomd.opensciencegrid.org/WLCGperfSONAR/check_mk/>

The [perfSONAR](http://docs.perfsonar.net/) toolkit is part of the [perfSONAR](http://www.perfsonar.net/) project. The current perfSONAR-PS toolkit is [available for download](http://docs.perfsonar.net/install_getting.html)

Ilija Vukotic/University of Chicago has setup an **analytics platform** (<https://twiki.cern.ch/twiki/bin/view/AtlasComputing/ATLASAnalytics>) using `Elastic Search` and `Kibana4`.

-    Here is the `Kibana4` web interface <http://cl-analytics.mwt2.org:5601/>

ESnet maintains an excellent set of pages about networking, end-system tuning, tools and techniques at <https://fasterdata.es.net/>

The are two related projects for OSG Networking

-   **PuNDIT** (an OSG Satellite project) focusing on analyzing perfSONAR data to alert on problems: <http://pundit.gatech.edu/>
-   **MadAlert** which analyzes **MaDDash** meshes to identify network problems: <http://madalert.aglt2.org/madalert/>

**Comments**
------------

<span class="twiki-macro COMMENT" type="tableappend"></span>

-- Main.ShawnMcKee - 09 Sep 2012
>>>>>>> 9493f7c69f4a04846a69a242b77e1da8452831f9

