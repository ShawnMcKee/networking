<<<<<<< HEAD
## perfSONAR Installation Guide 

This page documents installing/upgrading **perfSONAR** for OSG and WLCG sites. In case this is the first time you're trying to install and integrate your perfSONAR into WLCG or OSG, please consult our [overview](../perfsonar-in-osg.md) and possible [deployment options](deployment-models.md) before installing. For troubleshooting an existing installation please consult official [Troubleshooting Guide](http://docs.perfsonar.net/troubleshooting_overview.html), [FAQ](http://docs.perfsonar.net/FAQ.html) as well as WLCG/OSG specific [FAQ](faq.md).

For any questions or help with WLCG perfSONAR setup, please contact [GGUS](https://wiki.egi.eu/wiki/GGUS:WLCG_perfSONAR_FAQ) WLCG perfSONAR support unit or OSG [GOC](http://ticket.opensciencegrid.org/submit). We strongly recommend anyone maintaining/using perfSONAR to join [perfsonar-user](https://lists.internet2.edu/sympa/subscribe/perfsonar-user) and [perfsonar-announce](https://lists.internet2.edu/sympa/subscribe/perfsonar-announce) mailing lists.

### Installation

Prior to installing please consult the [release notes](https://www.perfsonar.net/) for the latest available release. In case you have already an instance running and wish to re-install/update it then please follow our recommendations:

* We recommend reinstalling using CentOS7 to all sites already running a registered instance or planning new installation. The primary reason for this recommendation is that the next point release of perfSONAR (4.1) will no longer support RHEL6/CentOS6/Scientific Linux 6.
* perfSONAR team provides support for Debian9 and Ubuntu as well, but we recommend to use CentOS7 as this is the most common and best understood deployment.
* Please backup `/etc/perfsonar/meshconfig-agent.conf`, which contains the current configuration.
* Local measurement archive backup is not needed as OSG/WLCG stores all measurements centrally. In case you'd like to perform the backup  anyway please follow the [migration guide](http://docs.perfsonar.net/install_migrate_centos7.html).
* In case you plan to deploy a single bare metal node with multiple NICs, please consult [Multiple NIC Guidance](deployment-models.md)

The following options are available to install perfSONAR toolkit:

| Installation method              | Link                                                                                    |
|----------------------------------|-----------------------------------------------------------------------------------------|
| Meta-package/bundle installation | [Bundle installation guide](http://docs.perfsonar.net/install_centos.html)              |
| Full ISO image installation      | [Toolkit full install guide](http://docs.perfsonar.net/install_centos_fullinstall.html) |
| Net ISO image installation       | [Toolkit NET install guide](http://docs.perfsonar.net/install_centos_netinstall.html)   |

!!! note
    In all cases, we **strongly recommend to keep auto-updates enabled** as this is the default settings starting from perfSONAR 4.0. With `yum` auto-updates in place there is a possibility that updated packages can "break" your perfSONAR install but this is viewed an acceptable risk in order to have security updates quickly applied on perfSONAR instances. 

The following *additional* steps are needed to configure the toolkit to be used in OSG/WLCG in addition to the steps described in the official guide:

* Please register your nodes in GOCDB/OIM. For OSG sites, follow the details in [OIM](#register-perfsonar-in-oim). For non-OSG sites, follow the details in [GOCDB](#register-perfsonar-service-in-gocdb)
* Please ensure you have added or updated your [administrative information](http://docs.perfsonar.net/manage_admin_info.html)
* You will need to configure your instance(s) to use the OSG/WLCG mesh-configuration. If this is a re-installation you can just revert from backup the file `/etc/perfsonar/meshconfig-agent.conf`. Otherwise please follow the steps below: 
    * Add a mesh section with configuration_url pointing to `http://psconfig.opensciencegrid.org/pub/auto/<FQDN>` Replace `<FQDN>` with the fully qualified domain name of your host, e.g., `psum01.aglt2.org`. Below is an example set of lines for `meshconfig-agent.conf`:
 
```
       <mesh> 
         configuration_url http://psconfig.opensciencegrid.org/pub/auto/psum01.aglt2.org
         validate_certificate 0 
         required 1 
       </mesh> 	
```

* If this is a **new instance** or you have changed the node's FQDN, you will need to notify `wlcg-perfsonar-support 'at' cern.ch` to add/update the hostname in one or more test meshes, which will then auto-configure the tests. Please indicate if you have preferences for which meshes your node should be included in (USATLAS, USCMS, ATLAS, CMS, LHCb, Alice, BelleII, etc.). You could also add any additional local tests  via web interface (see [Configuring regular tests](http://docs.perfsonar.net/manage_regular_tests.html) for details). Please check which tests are auto-added via central meshes before adding any custom tests to avoid duplication. 

!!! note
    Until your host is added (on http://psconfig.opensciencegrid.org ) to one or more meshes by a mesh-config administrator, the automesh configuration above won't be returning any tests (See registration information above).
	
* We **recommend** configuring perfSONAR in **dual-stack mode** (both IPv4 and IPv6). In case your site has IPv6 support, the only necessary step is to get both A and AAAA records for your perfSONAR DNS names (as well as ensuring the reverse DNS is in place).
* Adding *communities* is optional, but if you do, we recommend putting in WLCG as well as your VO: `ATLAS`, `CMS`, etc. This just helps others from the community lookup your instances in the public lookup service. As noted in the documentation you can select from already registered communities as appropriate.
* Please check that both **local and campus firewall** has the necessary [port openings](#security-considerations). Local iptables are configured automatically, but there are ways how to tune the existing set, please see the official [firewall](http://docs.perfsonar.net/manage_security.html#adding-your-own-firewall-rules) guide for details.
* Once installation is finished, please **reboot** the node.

For any further questions, please consult official [Troubleshooting Guide](http://docs.perfsonar.net/troubleshooting_overview.html), [FAQ](http://docs.perfsonar.net/FAQ.html) as well as WLCG/OSG specific [FAQ](faq.md) or contact directly WLCG or OSG perfSONAR support units.

### Maintenance

Provided that you have enabled auto-updates, the only thing that remains is to follow up on any kernel security issues and either patch the node as soon as possible or reboot once the patched kernel is released. perfSONAR team has dropped support for web100 kernel, which means that stock kernels from centOS7 as well as any updates can be deployed as soon as they're released.

In case you'd like to manually update the node please follow the official [guide](http://docs.perfsonar.net/manage_update.html).

Using automated configuration tools (such as Chef, Puppet, etc) for managing perfSONAR are not officially supported, but there are some community driven projects that could be helpful, such as [HEP-Puppet](http://github.com/HEP-Puppet/perfsonar). As perfSONAR manages most of its configuration automatically via packages and there is very little initial configuration needed, we suggest to keep automated configuration to the minimum necessary to avoid unncessary interventions after auto-updates. 

### Security Considerations

The perfSONAR toolkit is reviewed both internally and externally for security flaws and the official documentation provides a lot of information on what security software is available and what firewall ports need to be opened, please see [Manage Security](http://docs.perfsonar.net/manage_security.html) for details. The toolkit's purpose is to allow us to measure and diagnose network problems and we therefore need to be cautious about blocking needed functionality by site or host firewalls.

!!! warning 
	As of perfSONAR 4.0+ ALL perfSONAR instances need to have port 443 accessible to all the other perfSONAR instances. Allowing access to port 443 is needed because it's now used as a controller port for scheduling tests (via pScheduler). If sites are unable to reach your instance on port 443, tests may not run and results may not be available. The old test scheduler (BWCTL) will be retired in perfSONAR 4.1 release (planned Q1 2018) at which point access to port 443 will be the only way how to run the tests. Starting from perfSONAR 4.0, HTTPS/443 is now by default configured on all perfSONAR instances, i.e. local iptables as well as httpd configuration comes out of the box and requires no extra steps, therefore opening is only needed if you have central/campus firewall.

For sites that are concerned about having port 443 open, there is a possiblity to get a list of hosts to/from which the tests will be initiated. However as this list is dynamic, implementing the corresponding firewall rules would need to be done both locally and on the central/campus firewall in a way that would ensure dynamic updates. It's important to emphasize that port 443 provides access to the perfSONAR web interface as well, which is very useful to users and network administrators to debug network issues. 

!!! warning
	In case you have **central/campus firewall**, please check the required port openings in the [perfSONAR security documentation](http://docs.perfsonar.net/manage_security.html).  
	
### Enabling SNMP plugins

Starting from release 4.0.2, perfSONAR toolkit allows to configure passive SNMP traffic from the local routers to be captured and stored in the local measurement archive. This is currently a [beta feature](http://www.perfsonar.net/release-notes/version-4-0-2/) that needs further testing and we're looking for volunteers willing to test, please let us know in case you would be interested.

### Register perfSONAR Service in GOCDB

This section describes how to register the perfSONAR service in GOCDB.

In order to register you perfSONAR services in GOCDB, you should access the proper section of GOC for adding a Service Endpoint

-   <https://goc.egi.eu/portal/index.php?Page_Type=New_Service_Endpoint>

You might not be able to access the page if you are not properly registered in GOC, so a snapshot can be found below. In filling the information please follow those simple guidelines:

-   There are two service types for perfSONAR: net.perfSONAR.Bandwidth and net.perfSONAR.Latency. This is because we suggest t install two perfSONAR boxes at the site (one for latency tests and one for bandwidth tests) and therefore two distinct service endpoints should be published with two distinct service types. If the site can not afford sufficient hardware for the proposed setup, it can install a unique perfSONAR box, but still should publish both services types (with the same host in the "host name" field of the form).
-   For each form (i.e. for each service type) fill at least the important informations:
    -   Hosting Site (drop-down menu, mandatory)
    -   Service Type (drop-down menu, mandatory)
    -   Host Name (free text, mandatory)
    -   Host IP (free text, optional)
    -   Description: (free text, optional) This field has a default value of your site name. It is used to "Label" your host in our MaDDash GUI. If you want to use this field please use something as short as possible uniquely identifying this instance.
    -   Check "N" when asked "Is it a beta service"
    -   Check "Y" when asked "Is this service in production"
    -   Check "Y" when asked "Is this service monitored"

<!-- -->

-   GOCDB screen shot for creating a Service Endpoint: 
<img src="https://opensciencegrid.github.io/networking/img/Screen_shot_2013-02-19_at_15.26.52.png" width="1024">

### Register perfSONAR in OIM

!!! warning
	These instructions were revised on October 1, 2013.** If you registered your perfSONAR instances in OIM following instructions prior to this date please update them using the following instructions. The summary of the change: Instead of registering one resource per "Site" and then having two services (latency and bandwidth) for that resource, the new instructions are to register each site/service combination as one resource. For example a Tier-2 with two sites (A,B) will now need to register 4 resources: **latency\_A**, **latency\_B**, **bandwidth\_A** and **bandwidth\_B**. 


This section describes how to register your perfSONAR-PS instances in OIM. For general information about registering items in OIM see <https://twiki.opensciencegrid.org/bin/view/Operations/OIMRegistrationInstructions>

Each OSG site should have two perfSONAR instances (one for Latency and one for Bandwidth) installed to enable network monitoring. These instances should be located as "close" (in a network-sense) as possible to the site's storage. If a logical site is comprised of more than one physical site, each physical site should be instrumented with perfSONAR instances.

The example below uses AGLT2 (primarily an ATLAS Tier-2 center) which has two physical sites located at the University of Michigan in Ann Arbor and at Michigan State University in East Lansing.

To begin registration, make sure you have your X509 certificate loaded in your browser and go to

-   <https://oim.opensciencegrid.org> (for registered users)
-   <http://oim.opensciencegrid.org> (for guest)

-   The web page should be similar to this one with your site details. See notes on this screen capture: 

<img src="https://opensciencegrid.github.io/networking/img/OIM_perfSONAR_reg_1.png" width="1024">

-   You need to create a name for the new resource. This example is the latency instance at the AGLT2 UM site: 

<img src="https://opensciencegrid.github.io/networking/img/OSG-ps-register-latency.png" width="1024">

-   Please select the right service for the instance you are registering (either latency or bandwidth): 

<img src="https://opensciencegrid.github.io/networking/img/OSG-ps-register-latency-service.png" width="1024">

-   More details about registering the site. You need to include relevant contact details. These instructions will apply to all perfSONAR-PS instances you register: 

<img src="https://opensciencegrid.github.io/networking/img/OIM_perfSONAR_reg_4b.png" width="1024">

-   You need to agree to the OSG AUPs before submitting. These instructions will apply to each set of perfSONAR-PS instances you register: 

<img src="https://opensciencegrid.github.io/networking/img/OIM_perfSONAR_reg_4c.png" width="1024">

-   If you have another site/service to enter you should add another resource and create a new name (similar to above). For this example we also need to register **perfSONAR\_UM\_bandwidth** which is the perfSONAR bandwidth instance for the University of Michigan AGLT2 site. Since AGLT2 also has a site at Michigan State University we will also need to create two more resources/services: **perfSONAR\_MSU\_latency** and **perfSONAR\_MSU\_bandwidth**. <br />

-   This is similar what your "topology" should look like after **updating** to the new way of registering perfSONAR instances. Shown are both old and new entries. If you haven't registered before you won't see the old entries of course. 

<img src="https://opensciencegrid.github.io/networking/img/OSG-ps-register-topology.png" width="800">

After you have submitted each resource you should get an email confirming the requested registration with some details. **For sites that already have been running and registered in OIM there won't be much that you need to do.** In such a case you can choose to attend the next Monday meeting if you have questions or concerns. Details will be in the email that is sent. Also you should note that tickets will be created to track your new perfSONAR registrations. You don't have to take any action on them.

If you have problems or questions please consult our [FAQ](faq.md) or alternatively open a ticket with GOC. 
=======
<span class="twiki-macro LINKCSS"></span>

<span class="twiki-macro SPACEOUT">Installation and Upgrade Details</span>
==========================================================================


<span class="twiki-macro STARTINCLUDE"></span>

This page documents installing or upgrading **perfSONAR** for OSG and WLCG sites. In case this is the first time you're trying to install and integrate your perfSONAR into WLCG or OSG, please consult <https://twiki.opensciencegrid.org/bin/view/Documentation/PSMotivation> for an overview of perfSONAR.

For any questions or help with WLCG perfSONAR setup, please email `wlcg-perfsonar-support 'at' cern.ch` or open GGUS ticket for [WLCG perfSONAR Monitoring Support Unit](https://wiki.egi.eu/wiki/GGUS:WLCG_perfSONAR_FAQ). We strongly recommend anyone maintaining/using perfSONAR to join <https://lists.internet2.edu/sympa/subscribe/perfsonar-user> and <https://lists.internet2.edu/sympa/subscribe/perfsonar-announce>

Please follow the instructions for [upgrading](#UpgradeGuide) or [installing](#InstallingperfSONAR) perfSONAR as appropriate for your plans.

Updating perfSONAR
-------------------

Before updating, please read the release notes at <http://docs.perfsonar.net/manage_update.html#special-upgrade-notes>

To update please run the following:

    # yum clean all
    # yum update
    # reboot

Note, it's **very important to reboot** otherwise your update is incomplete. Please also note, that starting from v3.4 `yum` auto-updates are turned on by default. **We strongly recommend keeping them turned on**. With `yum` auto-updates in place there is a possibility that updated packages can "break" your perfSONAR install but this is viewed an acceptable risk in order to have security updates quickly applied on perfSONAR instances.

If you haven't yet added another (`non-root`) user to administer the web interface, you should run `/usr/lib/perfsonar/scripts/nptoolkit-configure.py` and do so, as suggested in `motd`. See <http://docs.perfsonar.net/manage_users.html> for detailed instructions.

Please backup your old `/etc/perfsonar/meshconfig-agent.conf` file and replace it with `/etc/perfsonar/meshconfig-agent.conf.rpmnew`.

Then please verify/reconfigure your boxes following [OSG and WLCG Configuration](#ConfigPS). For any further questions, please consult [Troubleshooting](https://twiki.opensciencegrid.org/bin/view/Documentation/TroubleFAQPS) pages, perfSONAR documentation (<http://docs.perfsonar.net>) or contact directly WLCG or OSG perfSONAR support units.

Installing perfSONAR
--------------------

perfSONAR 4.0 was released in April 2017. For sites running the previous version (3.x.y) the update should have automatically happened assuming that yum auto-updates were enabled and the `/etc/yum.repos.d/Internet2.repo` had 'enabled = 1'.

The following information details how to install perfSONAR 4.0 for WLCG and OSG. For general troubleshooting please go [here](https://twiki.opensciencegrid.org/bin/view/Documentation/TroubleFAQPS), in case you have any questions please email `wlcg-perfsonar-support 'at' cern.ch` or open GGUS ticket for [WLCG perfSONAR support unit](https://wiki.egi.eu/wiki/GGUS:WLCG_perfSONAR_FAQ). For OSG sites that don't participate in WLCG, please contact [OSG operations](https://twiki.opensciencegrid.org/bin/view/Operations/WebHome).

The perfSONAR team has created very good documentation on the process. You can get information about your installation options at <http://docs.perfsonar.net/install_getting.html#gettingchooseinstall>

### Installation

-   For sites using perfSONAR Net Install - please follow <http://docs.perfsonar.net/install_centos_netinstall.html#step-by-step-guide>
-   For sites using perfSONAR Full Install - please follow <http://docs.perfsonar.net/install_centos_fullinstall.html#step-by-step-guide>
-   For general configuration of perfSONAR - please follow <http://docs.perfsonar.net/install_config_first_time.html>

For sites the are currently registered but not yet updated to 4.0 we would strongly recommend reinstalling using CentOS 7.x (follow either the Full Install or the Net Install above). There is no need for sites to preserve their data from the OSG/WLCG perspective, since we have already been collecting your data centrally. The primary reason for this recommendation is that the next point release of perfSONAR (version 4.1) will no longer support RHEL6/CentOS6/Scientific Linux 6 as a supported operating system.

OSG and WLCG Configuration Notes
--------------------------------

All perfSONAR instances for use in OSG and WLCG should be registered either in OIM or GOCDB. Please verify or register your instances at:

-   For OSG, follow the details in [OIM](register-ps-in-oim)
-   For Non-OSG, follow the details in [GOCDB](register-ps-in-gocdb)

Please ensure you have added or updated your administrative information: <http://docs.perfsonar.net/manage_admin_info.html>

-   Adding communities is optional, but we recommend putting in WLCG as well as your VO: `ATLAS`, `CMS`, etc. This just helps others from the community lookup your instances in the lookup service. As noted in the documentation you can select from already registered communities as appropriate.

Assuming your registration is for a **new** node or if you have changed the nodes FQDN, you will need to email `wlcg-perfsonar-support 'at' cern.ch` or open GGUS ticket for [WLCG perfSONAR support unit](https://wiki.egi.eu/wiki/GGUS:WLCG_perfSONAR_FAQ) so that your node can be added to one or more meshes. Please indicate if you have preferences for which meshes your node should be included in.

You will need to configure your instance(s) to use the OSG/WLCG mesh-configuration. OSG provides MCA (Mesh Configuration Adminstrator) GUI (see <http://docs.perfsonar.net/mca.html> for details) to centrally define the network tests that need to be run. Each perfSONAR toolkit installation for OSG/WLCG should add the "auto" mesh URL in their `/etc/perfsonar/meshconfig-agent.conf` file:

-   Set this to `http://psconfig.opensciencegrid.org/pub/auto/<FQDN>` Replace `<FQDN>` with the fully qualified domain name of your host, e.g., `psum01.aglt2.org`.
-   ```
	<mesh> 
	    configuration_url <http://psconfig.opensciencegrid.org/pub/auto/psum01.aglt2.org>
	    validate_certificate 0 
	    required 1 
</mesh> 	
 # Replace the following with suitable values for your installation 
 address psum01.aglt2.org 
 admin_email <smckee@umich.edu> 
 skip_redundant_tests 1 
```

!!! note
	Until your host is added to one or more meshes, the automesh configuration above won't be returning any tests (See above).

We **strongly recommend** configuring perfSONAR in **dual-stack mode** (both IPv4 and IPv6)

-   In case your site has IPv6 support, the only necessary step is to get both A and AAAA records for your perfSONAR DNS names (as well as ensuring the reverse is in place).
-   All existing meshes will support both IPv4 and IPv6 testing. Sites with both IPv4 and IPv6 addresses testing to sites that also have both will run **two** tests. A side effect of this is that as more sites provide IPv6 addresses, the amount of testing will increase.
-   At some future point when most sites have IPv6, we may need to adjust the testing frequency to reduce the overall amount of testing
-   For more information on IPv6 see <http://ipv6.web.cern.ch/>

#### Security Considerations

**NOTE: As of the release of perfSONAR 4.0 ALL perfSONAR instances need to have port 443 access to all other perfSONAR instances.** This change is because of the new requirements introduced by pScheduler. If sites are unable to reach your instance on port 443, tests may not run and results may not be available.

The perfSONAR toolkit is reviewed both internally and externally for security flaws. The toolkit's purpose is to allow us to measure and diagnose network problems and we therefore need to be cautious about blocking needed functionality by site or host firewalls.

Some sites are concerned about having port 80 and/or 443 open. The working group would like to emphasize that these ports provide access to the perfSONAR web interface and are very useful to users and network administrators. **Our recommendation is to keep them open**, but for sites with strong concerns we have some rules documented below to customize iptables to block ports 80 and 443. It is **required** that either port 80 **or** port 443 be accessible from the OSG and WLCG monitoring subnets shown below. In addition port 443 **must** be accessible to all other perfSONAR instances that your node will test to. This is a new requirement as of the release of perfSONAR 4.0. <verbatim> \# Port 443 must be open iptables -I INPUT 4 -p tcp --dport 443 -j ACCEPT

\# Allow port 80 for specific monitoring subnets AT A MINIMUM (we recommend opening port 80 so others can view/access your perfSONAR Toolkit web GUI) \#OSG monitoring subnet iptables -I INPUT 4 -p tcp --dport 80 -s 129.79.53.0/24 -j ACCEPT \# CERN subnet iptables -I INPUT 4 -p tcp --dport 80 -s 137.138.0.0/17 -j ACCEPT \# Infrastructure monitoring (hosted at AGLT2) iptables -I INPUT 4 -p tcp --dport 80 -s 192.41.231.110/32 -j ACCEPT \# Replace <LOCAL\_SUBNET> appropriately to allow access from your local systems iptables -I INPUT 4 -p tcp --dport 80 -s <LOCAL\_SUBNET> -j ACCEPT \# Reject ONLY if your site policy requires this \#iptables -I INPUT 5 -p tcp --dport 80 -j REJECT

/sbin/service iptables save </verbatim>

In case you have **central/campus firewall**, please ensure the following ports are opened on it for all your perfSONAR hosts: <verbatim> \# General purpose ports needed to do perfSONAR measurements ICMP/type 255, NTP UDP port:123, TRACEROUTE/PING UDP ports: 33434-33634 OWAMP TCP port:861, UDP ports: 8760-9960 BWCTL TCP ports: 4823, 6001-6200, 5000-5900 UDP ports: 6001-6200, 5000-5900

\# HTTP and/or HTTPS ports are needed to access your perfSONAR web interface \# and the measurement archive and to check availability of your instances by the infrastructure monitoring \# As of the release of perfSONAR 4.0 port 443 **must** be accessible to all other perfSONAR instances and the WLCG monitoring subnets HTTPS port 443 open to ALL \# At a minimum either port 80 **must** be open to the WLCG monitoring subnets. \# Our recommendation is to have HTTP open to allow users and network admins access to the perfSONAR web. HTTP port 80 - open to ALL or at least for the following source subnets OSG\_NET (129.79.53.0/24), AGLT2\_NET (192.41.231.110/32) and CERN\_NET (137.138.0.0/17) </verbatim>

For any further questions, please consult [Troubleshooting](https://twiki.opensciencegrid.org/bin/view/Documentation/TroubleFAQPS) pages, perfSONAR documentation (<http://docs.perfsonar.net>) or contact [WLCG perfSONAR Monitoring Support Unit](https://wiki.egi.eu/wiki/GGUS:WLCG_perfSONAR_FAQ).

-

<span class="twiki-macro STOPINCLUDE"></span>

<span class="twiki-macro BOTTOMMATTER"></span>

-- Main.ShawnMcKee - 13-Mar-2016

>>>>>>> 9493f7c69f4a04846a69a242b77e1da8452831f9
