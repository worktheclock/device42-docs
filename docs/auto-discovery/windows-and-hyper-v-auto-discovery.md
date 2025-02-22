---
title: "Windows and Hyper-V Autodiscovery"
sidebar_position: 38
---

## Installation Prerequisites for Windows discovery

Prior to running a Windows Discovery, you must install an instance of the Windows Discovery Service (WDS) on at least one Windows system which will connect to the Device42 main appliance (MA) or a Remote Collector (RC). WDS can be downloaded from [the Auto-Discovery Software page](https://www.device42.com/autodiscovery/).

For WDS installation instructions and detailed information, visit the [Windows Discovery Service (WDS) installation](getstarted/installation/windows-discovery-service-installation.md) documentation.

Your OS must be at Windows 8.1, Windows Server 2012 R2 or above with the latest patches installed.

## Creating & running Windows WMI/WinRM discovery jobs

Navigate to the _Discovery_ menu and select _HyperVisors / \*Nix / Windows_, this section will allow you to setup and save multiple autodiscovery jobs for Windows, Hyper-V, and other platforms.

![](/assets/images/WEB-728_windows-hyper-v-ad-menu.png)

1. **Select _"Add Hypervisors/\*nix/Win for Autodiscovery"_ to setup a new Windows / Hyper-V Autodiscovery job.**
2. To specify **Windows WMI-based** or **Hyper-V** discovery, select **Windows** as the Platform.
    -  As of 18.08.00, you can select WinRM as the default method within the Windows discovery (pictured below) and the URL prefix and port will default.
3. Include a "Job Name" to identify this autodiscovery job: 

![](/assets/images/New-WinRM.png)

\*Classic WinRM could be deprecated in the future but there are currently no plans in place to do so.

### Windows/Hyper-V discovery options & definitions:

- **Job Name:** User-defined name for the job.

- **Remote Collector:** Select which remote collector you’d like to run the discovery job from – optional.

- **Job Debug Level:** Debug On for extra debug info, useful for a support ticket.

- **Discovery Target(s):** Specify FQDN or IP of discovery target(s). If using FQDN, Device42 must be configured to resolve the DNS.

- **Use Service Account Credentials:** Will use the current logged-in user of the system running WDS to perform WMI discovery.

- **Query domain controller to obtain a list of discovery devices:** Hides the Discovery Target(s) field. Target(s) to be discovered in this mode are instead defined by the result of chosen LDAP Criteria as returned by the specified Microsoft Windows Active Directory Domain or Domain Directory Server. (See "Query domain controller to obtain a list of discovery devices" below for more information.)

- **Collect database server information:** Select this option to discover MYSQL and Oracle database servers. (Displays a Database Username/Password(s) field if selected.)

- **ADM Sampling Interval:** Off or sampling interval in minutes or hours.

- **Enable Resource Utilization Tracking for Device(s):** Optionally enable collection of resource utilization metrics from discovered devices.

- **Resource Utilization Sampling Interval:** Set the interval for RU data collection (only in effect if RU Tracking is enabled).

- **Autodiscovery Schedule:** You can schedule the discovery to run at certain times.

### “Query domain controller to obtain list of discovery devices” option

Selecting the “Query domain controller to obtain list of discovery devices” option hides the “Server(s):” field. Target(s) to be discovered in this mode are instead defined by the result of chosen “LDAP Criteria” as returned by the specified Microsoft Windows Active Directory Domain or Domain Directory Server.

![](/assets/images/WEB-728_windows-hyper-v-ad-query-domain-ldap-custom-filter.png)

_Relevant fields when using this discovery mode are as follows:_

- **Use Service Account Credentials**: Use whatever account credentials the WDS service executing the discovery is running as for authentication.

- **Domain Server:** Hostname or IP address of the Active Directory server to run the LDAP query against.

- **LDAP username/password:** Authentication credentials used to execute the LDAP query against the specified domain controller/server.

- **Use FQDN:** Use Fully Qualified Domain Name (FQDN).

- **LDAP Criteria:** Choose the LDAP Query to execute against AD, the resultant list which will then be targeted for Windows Autodiscovery. Selecting _“Custom”_ allows specification of a custom LDAP filter/query.


"Query domain controller" LDAP Query Example:

```
(&(objectCategory=computer)(dNSHostName=d42sus.pvt))
```

...will search the domain server for all computers with DNS hostname - d42sus.pvt and autodiscover the matches.

### Discovery with Microsoft LAPS \[Local Admin Password Solution\]

Microsoft LAPS (Local Admin Password Solution) is a method of securing Active Directory member servers whereby the server's local admin password is randomly generated and stored as an attribute of that servers AD object in Active Directory. This password can then be looked up on demand via an Active Directory / LDAP query, and is often used to support scripted / automated actions that iterate through lists of AD member servers. If you are looking to [Download LAPS from Microsoft, click here](https://www.microsoft.com/en-us/download/details.aspx?id=46899). For more information on LAPS, see [this article from Microsoft](https://support.microsoft.com/en-us/topic/microsoft-security-advisory-local-administrator-password-solution-laps-now-available-may-1-2015-404369c3-ea1e-80ff-1e14-5caafb832f53 target=), or if you'd like to deploy LAPS, [you might find this "Deploying LAPS" guide on 'FlamingKeys.com'](https://flamingkeys.com/deploying-the-local-administrator-password-solution-part-1/) helpful.

Device42 now supports pulling credentials from LAPS when discovering Active Directory domain member servers that are using Microsoft LAPS \[Local Admin Password Solution\] to manage their local admin passwords. **You will see this option _only_ when you have checked "Query domain controller to obtain list of discovery devices"**; once the former is checked, you will see "Use LAPS (only Applies to WDS)".

Check the _"Use LAPS (only Applies to WDS)"_ checkbox to enable it; Ensure "Query domain controller to obtain list of discovery devices" is checked to reveal the option: 

![](/assets/images/WEB-728_windows-hyper-v-ad-query-domain-ldap-custom-filter-1.png)

**Relevant fields / requirements for using LAPS discovery mode are as follows**:

1. Check the "Query domain controller to obtain list of discovery devices" option.
2. Specify your domain server
3. Specify an LDAP user/pass credential combination with permission to query the LAPS password for each server
4. You must have at least one instance of WDS installed (A requirement for all Windows-based discoveries).
5. Choose "All Computers" or "All Servers" from the **LDAP Criteria** dropdown, or _Optionally, supply your own custom LDAP query_ by selecting "Custom Filter" from the dropdown:
6. Run your discovery!

## Additional Options

Scroll down the discovery job page to see additional job options including _Exclusions_, _Naming_, _Host Discovery,_ _Hypervisor Options_, _Software and Applications_, and _Miscellaneous_ options.

![](/assets/images/WEB-728_windows-hyper-v-ad-host-hypervisor-opts.png)

![](/assets/images/WEB-728_windows-hyper-v-ad-software-misc-opts-1.png)

* * *

## Scheduling discoveries

![Schedule](/assets/images/auto-discovery-schedule.png)

In the schedule section you can set as many different autodiscovery schedules as needed to cover your environment in the Schedules section. You can choose which days of the week and time.

### Last Status and Run Report

This is where you will see information about the last run status of the autodiscovery job.

Run Report will give you summary diagnostic details of stderr/stdout for the last discovery job.

* * *

## Device naming & duplicate device prevention

To prevent or correct any duplicate devices, you can control the format for names of devices added. For example: If running a VMWare Discovery in tandem with a Windows discovery, there may be differences in discovered device names. vSphere will often show the short hostname, but a WMI query against the OS will return server FQDNs.

Device42 tries to best match devices by Serial #, UUID, and then name. However, if duplicates are added, the options detailed in the following section will assist in correcting them.

### Configuring Windows hostname discovery details

![](/assets/images/WEB-728_windows-hyper-v-ad-naming-opts.png)

When running any discovery, you can add or update the Device Name of any targets with the host-name format of your choosing. Device42 can combine discovered host-name with the Domain Name \[Option: 'Host-name plus Domain Name'\]. You can also add the name as discovered (host-name) as an alias, rather than replace an existing device's name by choosing the option 'Host-name and add Host-name plus Domain Name as Alias'. This option adds devices with the host-name as discovered as the device name, and the FQDN as the alias. Full details below:

For naming options, the above mentioned Strip Domain Suffix option occurs prior to assigning the device name according to the naming option you choose. The naming options are as follows:

1. Hostname as Discovered
2. Hostname plus Domain Name
3. Hostname and add Hostname plus Domain Name as Alias
4. Hostname plus Domain Name and add Hostname as Alias

In order to set the Device Name to the FQDN of the device, make sure you select an option that includes “Hostname plus Domain Name”.

#### **Windows hostname discovery process:**

Device42 uses the following WMI query to obtain host & domain details on Windows servers:

select DNSHostName, Name, Manufacturer, Model, TotalPhysicalMemory, Domain, DomainRole from Win32\_ComputerSystem

From these results, we'll first look at DNSHostName for the device name, and if that is empty we'll use Name.

To obtain the domain name, we'll use the Domain value if the DomainRole value is one of the following: _WinDomainRoles.MemberWorkstation, WinDomainRoles.MemberServer, WinDomainRoles.PrimaryDomainController or WinDomainRoles.BackupDomainController._

#### **Strip domain suffix option:**

When selected, Device42 will drop everything after the first "." in the device name.

Note that your "Device Name Format" setting works in \*conjunction\* with the "strip domain suffix" setting. Therefore, if your server hostnames INCLUDE domain names \[e.g. server1.domain.com\] and you do not strip domain suffix, "Hostname plus Domain Name" will append the domain again: 'server1.domain.com.domain.com'. If your server hostnames are consistently named as you'd like them to appear, use "Hostname as Discovered". "Strip domain suffix" + "Hostname plus domain Name" work best together in environments with inconsistent naming convention, stripping all hostnames to short names, and then appending found domains.

* * *

## General considerations

### Permission requirements - WMI & Windows

_Windows permissions requirements are broken down into two parts:_

- **A) Minimum required permissions for discovery of Windows hosts/guests**
- **B) Minimum required permissions for ADM - discovery of services & application data for dependency mapping on Windows**

* * *

#### A) Windows Autodiscovery minimum permissions:

The following requirements represent the **minimum necessary user account permissions** that should allow Device42 to connect and discover a Windows host:

**1)** Ensure at least **Enable Account**, **Remote Enable**, and **Read Security**, as well as **WMI permissions** are applied to the autodiscovery user account and to the following WMI Namespaces and subnamespaces:

<table><tbody><tr><td width="20%"><ul><li>CIMV2</li><li>StandardCimv2</li><li>default</li><li>virtualization <small>(Hyper-V only)</small></li><li>virtualizationv2 <small>(Hyper-V only)</small></li></ul></td><td width="25%"><ul><li>MicrosoftIISv2 <small>(Only for IIS)</small></li><li>WebAdministration <small>(Only for IIS 7+)</small></li><li>MicrosoftSqlServer <small>(Only for SQLServer)</small></li><li>SMS <small>(Only for SCCM)</small></li><li>MSCluster <small>(Only used if 'Discover Cluster Information' is selected)</small></li></ul></td></tr></tbody></table>

\*\*Note for HyperV discovery against Windows Server2k12 and newer: _Should discovery fail due to a permissions error, you may need to add your Device42 discovery account to the built in HyperV administrators group due to changes made with the way Microsoft verifies permissions on these newer operating systems._

**2) Firewall Rules** to Enable:

- Windows Management Instrumentation (DCOM-In)
- Windows Management Instrumentation (WMI-In)

**3) Services** to Enable and Set to Automatic:

- Remote Procedure Call (RPC)
- Windows Management Instrumentation

**4)** The discovery user account must be a member of the **Performance Monitor Users Group** and **Distributed COM Users Group** on the machines being targeted for discovery.

Provided the above is successfully configured for the discovery account (& the data is available), Device42 will successfully gather the following info:

<table>
    <tbody>
        <tr>
            <td width="20%">
                <ul>
                    <li>Device Host information</li>
                    <li>Operating System</li>
                    <li>Service Process</li>
                </ul>
            </td>
            <td width="25%">
                <ul>
                    <li>Software Installed</li>
                    <li>Installed common Applications and Configuration Files</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

Note: If you are discovering servers that do not belong to a domain, there may be issues due to UAC settings. Please refer to this [MSDN article](https://learn.microsoft.com/en-us/windows/win32/wmisdk/user-account-control-and-wmi?redirectedfrom=MSDN) for information regarding UAC's effects on WMI.

* * *

#### B) Windows Application Dependency Mapping (ADM) minimum permissions:

There are two options for configuring Application Dependency Mapping permissions. The first option uses Local Administrative Permissions and the IPC$ & ADMIN$ shares. The second option lets users configure their own share**.**

**Local Administrator Method**:

1. Application Dependency Mapping **requires the local admin group on the target hosts and access to IPC & ADMIN shares.**
2. Enable **access to IPC & ADMIN shares**: Device42 will utilize the `IPC` and `ADMIN` shares to obtain service port communication details in Windows discovery. Ensure these shares are enabled and accessible over port 445, or inter-device communications will not be discovered.
3. Connection data is written to a path on the `admin$` administrative share, which is only writable by the local admin group. This path is used because it is a standard, consistent path that can be found on all Windows deployments.

**Alternate Method:**

When setting up the discovery job if the IPC and ADMIN shares are inaccessible you can now specify a network share to use. The share can be local to the device or a shared location on your network. You will need to give the scanning account read and write privileges to the new share location

![](/assets/images/Screen-Shot-2022-05-23-at-10.18.26-AM.png)

#### C) Port Matrix

| Ports    | Protocol | Application Protocol      | Notes                                                                   |
| -------- | -------- | ------------------------- | ----------------------------------------------------------------------- |
| 135      | TCP      | WMI                       | Always required.                                                        |
| 137      | UDP      | NetBIOS Name Resolution    | Optional/Legacy. Windows 2000 and newer versions of Windows can work over port 445. |
| 138      | UDP      | NetBIOS Datagram Service   | Optional/Legacy. Windows 2000 and newer versions of Windows can work over port 445. |
| 139      | TCP      | SMB                       | Optional/Legacy. Windows 2000 and newer versions of Windows can work over port 445. |
| 445      | TCP      | SMB                       | Optional. Used by the WDS (Windows Discovery Service) to retrieve UDP communication and configuration files from targets. |
| 1024-5000| TCP      | RPC randomly allocated low TCP ports | Optional/Legacy. Used in Windows 2000, Windows XP, and Windows Server 2003. Newer versions of Windows use high TCP ports 49152 - 65535. |
| 49152-65535 | TCP   | RPC randomly allocated high TCP ports | Always required (Unless entire environment predates Windows Server 2008). Used in Windows Server 2008 and later versions, and in Windows Vista and later versions. |

* * *

### Best practices & limitations:

- If you've populated Device42 \[CSV imports, spreadsheets, manual entry, etc.\] with devices before your first discovery run, please make sure to test discovery against just a few devices to ensure the selected discovery naming options are correct for your naming convention. (For example, if you added nh-linux01 as a device, autodiscovery could find the hostname nh-linux01.example.com and add it as a new device because the names don't match. Fix this by checking the checkbox "use only hostname".)
- It's best to **run device autodiscovery after you have run network autodiscovery** and/or defined the subnets your network IPs reside on.
- Floating IPs that belong to a cluster logically, but are found on a device during autodiscovery will be assigned to that device and _not_ the cluster resource.
- You can run the WDS from any (and/or multiple) network segments. Communication from the autodiscovery client back to the main Device42 instance requires access via port TCP/443 (HTTPS) to be allowed on your network.

* * *

### Hyper-V information

To run a Hyper-V discovery, set the "Platform" to Windows, and enable the "Discover VMs" option.

The Hyper-V server's hardware details and its name, UUID, and MAC addresses are pulled in from the VMs. _Be sure_ to include the Hyper-V servers as hostname's, IP Address, or part of the CIDR/IP Range in the "Server(s)" criteria

* * *

### Windows 2000 discovery prerequisites

If you are looking to discover a legacy Windows 2000-based operating system, a few OS settings need to be tweaked on the machine hosting your WDS (Windows Discovery Service) to obtain proper results:

1. Change or create HKLM\\SYSTEM\\CurrentControlSet\\Control\\Lsa\\LmCompatibilityLevel element with value 1.
2. Change WDS service user from System to one of the host users _(admin)_. You can try without this step, but users report failure without making this change as well.
