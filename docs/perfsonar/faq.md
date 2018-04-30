<<<<<<< HEAD
### Frequently Asked Questions 

Here we will provide details on troubleshooting perfSONAR installations for OSG and WLCG as well as some additional configuration options and a FAQ.

A good overview of existing tools provided by perfSONAR toolkit and examples how to use them to identify and isolate network problems can be found at https://fasterdata.es.net/performance-testing/troubleshooting/network-troubleshooting-quick-reference-guide/

We are maintaining a [Network Troubleshooting](../network-troubleshooting.md) page to guide users in identifying and following up on network problems.

#### Network Troubleshooting

* I suspect there is a network performance issue impacting my site

For OSG sites, please open a ticket with GOC. Otherwise please open a GGUS ticket (or assign an existing) one to WLCG Network Throughput support unit.

#### Service Registration
**I got an email after registering with lots of information in it...what do I do?** 

This is part of the process. If you are a new site you will need to attend the next OSG operations meeting. If you are an existing site and have just registered perfSONAR instances you don't have to do anything but feel free to attend the next operations meeting if you have questions or concerns.

**Once I registered, new tickets were opened concerning perfSONAR...What do I do?** 

This is standard operating procedure and the tickets are to ensure that OSG operations properly gets your new perfSONAR instances registered. You don't have to do anything and the tickets will be closed by OSG operations staff.

#### Infrastructure Monitoring (check\_mk metrics)

* **perfSONAR services: versions** metric is failing.

This metrics checks if your sonar is at the most recent version. Please check if you have automatic yum updates enabled, this is strongly recommended due to security issues we have seen in the past. In case you're still running an older version (3.3-3.5), please update and reconfigure as soon as possible following [Installation Guide](installation.md) 

* **perfSONAR configuration: contacts or location** metrics are failing 

Please check if you have added the administrative information as detailed [here](http://docs.perfsonar.net/install_config_first_time.html#updating-your-administrative-information)

* **perfSONAR services: bwctl/owamp/pscheduler** metrics are failing

This means that we're unable to connect to controller ports of the respective services, please ensure you have correct firewall settings (especially white listed subnets allowed) as described in the [Installation Guide](installation.md) . This can also indicate failures of service daemons, please check <http://www.perfsonar.net/about/faq> for additional details.

* **perfSONAR services: esmond** metric is failing

This means that your measurement archive is not accessible or failing, there can be many possible causes (disk full, httpd not running or inaccessible, etc.), you can ask for help by opening a GGUS ticket to WLCG perfSONAR support. 

* **perfSONAR json summary** is failing
    -   This means the toolkit's homepage is inaccessible, which is required to check many additional services, so in turn all the other metrics will likely be in unknown or critical state. Please check for usual causes (disk full, httpd not running or blocked), we need to be able to access your homepage via HTTP or HTTPS 
    
* **perfSONAR configuration: meshes** metric is failing

This indicates that you're missing the recommended mesh configuration. Please follow mesh configuration as detailed in the [installation guide](installation.md). Also, please REMOVE any old mesh configuration, this metric will also fail in case you have both the new mesh config and the old mesh URLs 

* **perfSONAR services: ntp** is failing

This indicates that NTP service is not running correctly on your sonar, please note that NTP is critical service. Please refer to <http://www.perfsonar.net/about/faq/>

* **perfSONAR services: regular testing/pscheduler** is failing

This indicates that pscheduler is not working correctly. As this is the core daemon please contact WLCG perfSONAR support unit for help.

* There are **many tests failing** for given sonar, where should I start

Please update and reconfigure your sonar following [Installation Guide](installation.md). Please ensure firewall doesn't block access from the whitelisted subnets that are required for the infrastructure monitoring to work. 

* **Where can I get support on managing WLCG perfSONAR** ?

You can open ticket in GGUS to WLCG perfSONAR support unit or contact directly wlcg-perfsonar-support (at cern.ch) 

* **perfSONAR esmond freshness Latency/Bandwidth Direct** is failing or gives warning

This metric checks freshness of the local measurement archive, in particular it checks if it contains fresh results for all the configured tests. This metric is needed to determine if we're able to consistently get results from perfSONAR boxes in WLCG. Currently it's a non-critical test, you can ignore it. 

* **perfSONAR services ndt/npad** is failing

Both metrics check if you have disabled NDT and NPAD. As both NDT and NPAD have been dropped starting with 4.0, this metrics should stay green in most of the cases. 

* **perfSONAR hardware check** is failing

Please consult the minimum and recommended [hardware requirements](deployment-models.md). 
=======
<span class="twiki-macro LINKCSS"></span>

<span class="twiki-macro SPACEOUT">perfSONAR  FAQ and  Troubleshooting  for  OSG  and  WLCG  Instances</span>
=============================================================================================================


<span class="twiki-macro STARTINCLUDE"></span> Here we will provide details on troubleshooting perfSONAR installations for OSG and WLCG as well as some additional configuration options and a FAQ.

There are a good set of `cli` tools for perfSONAR available. Details on these tools are at <https://twiki.opensciencegrid.org/bin/view/Documentation/Release3/NetworkPerformanceToolkit> The `owping` and `bwctl` tools can be very useful to test from your location to any perfSONAR instance running either the OWAMP or BWCTL services respectively.

We are maintaining a [Network Troubleshooting](https://twiki.opensciencegrid.org/bin/view/Documentation/NetworkingTroubleShooting) Wiki page to guide users in identifying and following up on network problems.

To further secure access to your perfSONAR web interface, we are providing a [securing the perfSONAR web](SecureperfSONAR) page (not yet tested).

FAQ
---

### My perfSONAR disks are filling up with log messages.

The default logging in perfSONAR 3.4 produces a lot of output and some sites are filling their disks with logging data. This has been reported to the perfSONAR developers and there will be fixes coming in future versions. In the meantime you can reduce the amount of logging by doing the following. As `root` you can locate all the relevant logger.conf files:

    [root@psum01 ~]# locate logger.conf
    /opt/SimpleLS/bootstrap/etc/SimpleLSBootStrapClientDaemon-logger.conf
    /opt/perfsonar_ps/ls_cache_daemon/etc/ls_cache_daemon-logger.conf
    /opt/perfsonar_ps/ls_registration_daemon/etc/ls_registration_daemon-logger.conf
    /opt/perfsonar_ps/regular_testing/etc/regular_testing-logger.conf
    /opt/perfsonar_ps/toolkit/etc/config_daemon-logger.conf
    /opt/perfsonar_ps/toolkit/etc/service_watcher-logger.conf
    /opt/perfsonar_ps/toolkit/web/root/admin/administrative_info/etc/logger.conf
    /opt/perfsonar_ps/toolkit/web/root/admin/enabled_services/etc/logger.conf
    /opt/perfsonar_ps/toolkit/web/root/admin/ntp/etc/logger.conf
    /opt/perfsonar_ps/toolkit/web/root/admin/regular_testing/etc/logger.conf
    /opt/perfsonar_ps/toolkit/web/root/gui/services/etc/logger.conf

You can move the logging levels for DEBUG and INFO up to WARN level via:

    sed -i 's/DEBUG,/INFO,/g' `locate logger.conf`
    sed -i 's/INFO,/WARN,/g' `locate logger.conf`

Comment out the 'verbose' line from `/etc/owampd/owampd.conf`

Finally reboot:

    reboot

### Infrastructure Monitoring (check\_mk metrics)

1.  **perfSONAR 3.4+ Toolkit Version** metric is failing
    -   This metrics checks if your sonar is at particular version (as of Dec. 12, it checks if it's at 3.4.1). In case you're at version 3.4 (and not 3.4.1), please check if you have automatic yum updates enabled, this is strongly recommended due to security issues we have seen in the past. In case you're still running version an older version (3.3), please update and reconfigure as soon as possible following [Installation Guide](https://twiki.opensciencegrid.org/bin/view/Documentation/InstallUpdatePS) 2. **perfSONAR Administrator Details** metric is failing 3. **perfSONAR BWCTL Bandwidth Test Controller** metric is failing
    -   This means that we're unable to connect to your bandwidth controller port, please ensure you have correct firewall settings (especially white listed subnets allowed) as described in the [Installation Guide](https://twiki.opensciencegrid.org/bin/view/Documentation/InstallUpdatePS) . This can also indicate failures of bwctl daemon, please check <http://www.perfsonar.net/about/faq> (e.g. <http://www.perfsonar.net/about/faq/#Q22>), you can ask for help by opening a GGUS ticket to WLCG perfSONAR support 4. **perfSONAR esmond Measurement Archive** metric is failing
    -   This means that your measurement archive is not accessible, there can be many possible causes (disk full, httpd not running or inaccessible, etc.), you can ask for help by opening a GGUS ticket to WLCG perfSONAR support. 5. **perfSONAR Homepage** is failing
    -   This means the toolkit's homepage is inaccessible, please check for usual causes (disk full, httpd not running or blocked), we need to be able to access your homepage via HTTP or HTTPS 7. **perfSONAR Latitude/Longitude Configured** is failing 8. **perfSONAR Mesh Configuration** is failing
    -   This indicates that you're missing the recommended mesh configuration. Please configure your mesh URL(s) in **/opt/perfsonar\_ps/mesh\_config/etc/agent\_configuration.conf**. We have a new `auto-mesh` capability now and all sites should set:
    -   Set this to `http://meshconfig.opensciencegrid.org/pub/auto/<FQDN>` Replace `<FQDN>` with the fully qualified domain name of your host, e.g., `psum01.aglt2.org`. Values for each instance are [in this list](http://grid-monitoring.cern.ch/perfsonar_config.txt).
    -   <verbatim>

<mesh> configuration\_url <http://meshconfig.opensciencegrid.org/pub/auto/psum01.aglt2.org> validate\_certificate 0 required 1 </mesh> </verbatim>

-   Please REMOVE any old mesh configuration, this metric will also fail in case you have both the new mesh config and the old mesh URLs 9. **perfSONAR NTP Service** is failing
-   This indicates that NTP service is not running correctly on your sonar, please note that NTP is critical service. Please refer to <http://www.perfsonar.net/about/faq/> 10. **perfSONAR Regular Testing Service** is failing
-   This indicates that regular testing service is not working, please try to enable/disable it following <http://docs.perfsonar.net/manage_regular_tests.html#disabling-enabling-regular-tests>
-   In case the issue persists, please open ticket to WLCG perfSONAR SU in GGUS 11. **perfSONAR Toolkit Version** is failing
-   This metrics checks if your sonar is at particular version (as of Dec. 12, it checks if it's at 3.4.1). In case you're at version 3.4 (and not 3.4.1), please check if you have automatic yum updates enabled, this is strongly recommended due to security issues we have seen in the past. In case you're still running version an older version (3.3), please update and reconfigure as soon as possible following [Installation Guide](https://twiki.opensciencegrid.org/bin/view/Documentation/InstallUpdatePS) 12. There are **many tests failing** for given sonar, where should I start
-   Please update and reconfigure your sonar following [Installation Guide](https://twiki.opensciencegrid.org/bin/view/Documentation/InstallUpdatePS). Please ensure firewall doesn't block access from the whitelisted subnets that are required for the infrastructure monitoring to work. 13. **Where can I get support on managing WLCG perfSONAR**
-   You can open ticket in GGUS to WLCG perfSONAR support unit or contact directly wlcg-perfsonar-support (at cern.ch) 14. **perfSONAR esmond Freshness Latency/Bandwidth Direct** is failing or gives warning
-   This metric checks freshness of the local measurement archive, in particular it checks if it contains fresh results for all the configured tests. This metric is needed to determine if we're able to consistently get results from perfSONAR boxes in WLCG. Currently it's a non-critical test, you can ignore it. 15. **perfSONAR esmond Freshness Latency/Bandwidth Reverse** is failing or gives warning
-   This metric checks freshness of the local measurement archive, in particular it checks if it contains fresh results for all the configured reverse tests (reverse test is any test that is done from remote to local sonar). This metric is needed to determine if we're able to consistently get results from perfSONAR boxes in WLCG. Currently it's a non-critical test, you can ignore it. 16. **perfSONAR NDT HTTP Network Diagnostic Tester** and/or **perfSONAR NDT P Network Diagnostic Tester** and/or **perfSONAR NPAD Network Path and Application Diagnosis** are failing
-   Both metrics check if you have disabled NDT and NPAD as we recommend in the [Installation/Update Guide](https://twiki.opensciencegrid.org/bin/view/Documentation/InstallUpdatePS). Please follow the guide to disable this service. 17. **perfSONAR Toolkit Memory** is failing
-   Starting with perfSONAR version 3.4, the minimal required memory is at least 4GBs. In case hardware update is not possible for your sonars, we can still use them to run on demand tests for debugging purposes, but we won't be able to run any baselining activities.

<span class="twiki-macro STOPINCLUDE"></span>

<span class="twiki-macro BOTTOMMATTER"></span>

-- Main.ShawnMcKee - 16 Oct 2014

>>>>>>> 9493f7c69f4a04846a69a242b77e1da8452831f9
