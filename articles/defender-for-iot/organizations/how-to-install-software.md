---
title: Defender for IoT installation
description: Learn how to install a sensor and the on-premises management console for Azure Defender for IoT.
ms.date: 10/21/2021
ms.topic: how-to
---

# Defender for IoT installation

This article describes how to install the following Azure Defender for IoT components:

- **Sensor**: Defender for IoT sensors collects ICS network traffic by using passive (agentless) monitoring. Passive and nonintrusive, the sensors have zero impact on OT and IoT networks and devices. The sensor connects to a SPAN port or network TAP and immediately begins monitoring your network. Detections appear in the sensor console. There, you can view, investigate, and analyze them in a network map, device inventory, and an extensive range of reports. Examples include risk assessment reports, data mining queries, and attack vectors.

- **On-premises management console**: The on-premises management console lets you carry out device management, risk management, and vulnerability management. You can also use it to carry out threat monitoring and incident response across your enterprise. It provides a unified view of all network devices, key IoT, and OT risk indicators and alerts detected in facilities where sensors are deployed. Use the on-premises management console to view and manage sensors in air-gapped networks.

This article covers the following installation information:

- **Hardware:** Dell and HPE physical appliance details.

- **Software:** Sensor and on-premises management console software installation.

- **Virtual Appliances:** Virtual machine details and software installation.

After installation, connect your sensor to your network.

## About Defender for IoT appliances

The following sections provide information about Defender for IoT sensor appliances and the appliance for the Defender for IoT on-premises management console.

### Physical appliances

The Defender for IoT appliance sensor connects to a SPAN port or network TAP and immediately begins collecting ICS network traffic by using passive (agentless) monitoring. This process has zero impact on OT networks and devices because it isn't placed in the data path and doesn't actively scan OT devices.

The following rack mount appliances are available:

| **Deployment type** | **Corporate** | **Enterprise** | **SMB** |**SMB Ruggedized** |
|--|--|--|--|--|
| **Model** | HPE ProLiant DL360 | HPE ProLiant DL20 | HPE ProLiant DL20 | HPE EL300 |
| **Monitoring ports** | up to 15 RJ45 or 8 OPT | up to 8 RJ45 or 6 OPT | up to 4 RJ45 | Up to 5 RJ45 |
| **Max Bandwidth\*** | 3 Gb/Sec | 1 Gb/Sec | 200 Mb/Sec | 100 Mb/Sec |
| **Max Protected Devices** | 10,000 | 10,000 | 1,000 | 800 |

*Maximum bandwidth capacity might vary depending on protocol distribution.

### Virtual appliances

The following virtual appliances are available:

| **Deployment type** | **Corporate** | **Enterprise** | **SMB** |
|--|--|--|--|
| **Description** | Virtual appliance for corporate deployments | Virtual appliance for enterprise deployments | Virtual appliance for SMB deployments |
| **Max Bandwidth\*** | 2.5 Gb/Sec | 800 Mb/sec | 160 Mb/sec |
| **Max protected devices** | 10,000 | 10,000 | 800 |
| **Deployment Type** | Corporate | Enterprise | SMB |

*Maximum bandwidth capacity might vary depending on protocol distribution.

### Hardware specifications for the on-premises management console

 | Item | Description |
 |----|--|
 **Description** | In a multi-tier architecture, the on-premises management console delivers visibility and control across geographically distributed sites. It integrates with SOC security stacks, including SIEMs, ticketing systems, next-generation firewalls, secure remote access platforms, and the Defender for IoT ICS malware sandbox. |
 **Deployment type** | Enterprise |
 **Appliance type**  | Dell R340, VM |
 **Number of managed sensors** | Unlimited |

## Prepare for the installation

### Access the ISO installation image

The installation image is accessible from the Defender for IoT portal.

To access the file:

1. Sign in to your Defender for IoT account.

1. Go to the **Network sensor** or **On-premises management console** page and select a version to download.

### Install from DVD

Before the installation, ensure you have:

- A portable DVD drive with the USB connector.

- An ISO installer image.

To install:

1. Burn the image to a DVD or prepare a disk on a key. Connect a portable DVD drive to your computer, right-click the ISO image, and select **Burn to disk**.

1. Connect the DVD or disk on a key and configure the appliance to boot from DVD or disk on a key.

### Install from disk on a key

Before the installation, ensure you have:

- Rufus installed.
  
- A disk on key with USB version 3.0 and later. The minimum size is 4 GB.

- An ISO installer image file.

The disk on a key will be erased in this process.

To prepare a disk on a key:

1. Run Rufus and select **SENSOR ISO**.

1. Connect the disk on a key to the front panel.

1. Set the BIOS of the server to boot from the USB.

## Dell PowerEdgeR340XL installation

Before installing the software on the Dell appliance, you need to adjust the appliance's BIOS configuration:

- [Dell PowerEdge R340 Front Panel](#dell-poweredge-r340-front-panel) and [Dell PowerEdge R340 Back Panel](#dell-poweredge-r340-back-panel) contains the description of front and back panels, along with information required for installation, such as drivers and ports.

- [Dell BIOS Configuration](#dell-bios-configuration) provides information about how to connect to the Dell appliance management interface and configure the BIOS.

- [Software Installation (Dell R340)](#software-installation-dell-r340) describes the procedure required to install the Defender for IoT sensor software.

### Dell PowerEdge R340XL requirements

To install the Dell PowerEdge R340XL appliance, you need:

- Enterprise license for Dell Remote Access Controller (iDrac)

- BIOS configuration XML

- Server firmware versions:

  - BIOS version 2.1.6

  - iDrac version 3.23.23.23

### Dell PowerEdge R340 front panel

:::image type="content" source="media/tutorial-install-components/view-of-dell-poweredge-r340-front-panel.jpg" alt-text="Dell PowerEdge R340 front panel.":::

 1. Left control panel
 1. Optical drive (optional)
 1. Right control panel
 1. Information tag
 1. Drives  

### Dell PowerEdge R340 back panel

:::image type="content" source="media/tutorial-install-components/view-of-dell-poweredge-r340-back-panel.jpg" alt-text="Dell PowerEdge R340 back panel.":::

1. Serial port
1. NIC port (Gb 1)
1. NIC port (Gb 1)
1. Half-height PCIe
1. Full-height PCIe expansion card slot
1. Power supply unit 1
1. Power supply unit 2
1. System identification
1. System status indicator cable port (CMA) button
1. USB 3.0 port (2)
1. iDRAC9 dedicated network port
1. VGA port

### Dell BIOS configuration

Dell BIOS configuration is required to adjust the Dell appliance to work with the software.

The Dell appliance is managed by an integrated iDRAC with Lifecycle Controller (LC). The LC is embedded in every Dell PowerEdge server and provides functionality that helps you deploy, update, monitor, and maintain your Dell PowerEdge appliances.

To establish the communication between the Dell appliance and the management computer, you need to define the iDRAC IP address and the management computer's IP address on the same subnet.

When the connection is established, the BIOS is configurable.

**To configure Dell BIOS**:

1. [Configure the iDRAC IP address](#configure-idrac-ip-address)

1. [Configuring the BIOS](#configuring-the-bios)

#### Configure iDRAC IP address

1. Power up the sensor.

1. If the OS is already installed, select the F2 key to enter the BIOS configuration.

1. Select **iDRAC Settings**.

1. Select **Network**.

   > [!NOTE]
   > During the installation, you must configure the default iDRAC IP address and password mentioned in the following steps. After the installation, you change these definitions.

1. Change the static IPv4 address to **10.100.100.250**.

1. Change the static subnet mask to **255.255.255.0**.

   :::image type="content" source="media/tutorial-install-components/idrac-network-settings-screen-v2.png" alt-text="Screenshot that shows the static subnet mask.":::

1. Select **Back** > **Finish**.

#### Configuring the BIOS

You need to configure the appliance BIOS if:

- You did not purchase your appliance from Arrow.

- You have an appliance, but do not have access to the XML configuration file.

After you access the BIOS, go to **Device Settings**.

**To configure the BIOS**:

1. Access the appliance's BIOS directly by using a keyboard and screen, or use iDRAC.

   - If the appliance is not a Defender for IoT appliance, open a browser and go to the IP address that was configured before. Sign in with the Dell default administrator privileges. Use **root** for the username and **calvin** for the password.

   - If the appliance is a Defender for IoT appliance, sign in by using **XXX** for the username and **XXX** for the password.

1. After you access the BIOS, go to **Device Settings**.

1. Choose the RAID-controlled configuration by selecting **Integrated RAID controller 1: Dell PERC\<PERC H330 Adapter\> Configuration Utility**.

1. Select **Configuration Management**.

1. Select **Create Virtual Disk**.

1. In the **Select RAID Level** field, select **RAID5**. In the **Virtual Disk Name** field, enter **ROOT** and select **Physical Disks**.

1. Select **Check All** and then select **Apply Changes**

1. Select **Ok**.

1. Scroll down and select **Create Virtual Disk**.

1. Select the **Confirm** check box and select **Yes**.

1. Select **OK**.

1. Return to the main screen and select **System BIOS**.

1. Select **Boot Settings**.

1. For the **Boot Mode** option, select **BIOS**.

1. Select **Back**, and then select **Finish** to exit the BIOS settings.

### Software installation (Dell R340)

The installation process takes about 20 minutes. After the installation, the system is restarted several times.

To install:

1. Verify that the version media is mounted to the appliance in one of the following ways:

   - Connect the external CD or disk on a key with the release.

   - Mount the ISO image by using iDRAC. After signing in to iDRAC, select the virtual console, and then select **Virtual Media**.

1. In the **Map CD/DVD** section, select **Choose File**.

1. Choose the version ISO image file for this version from the dialog box that opens.

1. Select the **Map Device** button.

   :::image type="content" source="media/tutorial-install-components/mapped-device-on-virtual-media-screen-v2.png" alt-text="Screenshot that shows a mapped device.":::

1. The media is mounted. Select **Close**.

1. Start the appliance. When you're using iDRAC, you can restart the servers by selecting the **Consul Control** button. Then, on the **Keyboard Macros**, select the **Apply** button, which will start the Ctrl+Alt+Delete sequence.

1. Select **English**.

1. Select **SENSOR-RELEASE-\<version\> Enterprise**.

   :::image type="content" source="media/tutorial-install-components/sensor-version-select-screen-v2.png" alt-text="Select your sensor version and enterprise type.":::

1. Define the appliance profile, and network properties:

   :::image type="content" source="media/tutorial-install-components/appliance-profile-screen-v2.png" alt-text="Screenshot that shows the appliance profile, and network properties.":::

   | Parameter | Configuration |
   |--|--|
   | **Hardware profile** | **enterprise** |
   | **Management interface** | **eno1** |
   | **Network parameters (provided by the customer)** | - |
   |**management network IP address:** | - |
   | **subnet mask:** | - |
   | **appliance hostname:** | - |
   | **DNS:** | - |
   | **default gateway IP address:** | - |
   | **input interfaces:** |  The system generates the list of input interfaces for you. To mirror the input interfaces, copy all the items presented in the list with a comma separator. You do not have to configure the bridge interface. This option is used for special use cases only. |

1. After about 10 minutes, the two sets of credentials appear. One is for a **CyberX** user, and one is for a **Support** user.  

1. Save the appliance ID and passwords. You'll need these credentials to access the platform the first time you use it.

1. Select **Enter** to continue.

## HPE ProLiant DL20 installation

This section describes the HPE ProLiant DL20 installation process, which includes the following steps:

- Enable remote access and update the default administrator password.
- Configure BIOS and RAID settings.
- Install the software.

### About the installation

- Enterprise and SMB appliances can be installed. The installation process is identical for both appliance types, except for the array configuration.
- A default administrative user is provided. We recommend that you change the password during the network configuration process.
- During the network configuration process, you'll configure the iLO port on network port 1.
- The installation process takes about 20 minutes. After the installation, the system is restarted several times.

### HPE ProLiant DL20 front panel

:::image type="content" source="media/tutorial-install-components/hpe-proliant-dl20-front-panel-v2.png" alt-text="HPE ProLiant DL20 front panel.":::

### HPE ProLiant DL20 back panel

:::image type="content" source="media/tutorial-install-components/hpe-proliant-dl20-back-panel-v2.png" alt-text="The back panel of the HPE ProLiant DL20.":::

### Enable remote access and update the password

Use the following procedure to set up network options and update the default password.

To enable and update the password:

1. Connect a screen and a keyboard to the HP appliance, turn on the appliance, and press **F9**.

    :::image type="content" source="media/tutorial-install-components/hpe-proliant-screen-v2.png" alt-text="Screenshot that shows the HPE ProLiant window.":::

1. Go to **System Utilities** > **System Configuration** > **iLO 5 Configuration Utility** > **Network Options**.

    :::image type="content" source="media/tutorial-install-components/system-configuration-window-v2.png" alt-text="Screenshot that shows the System Configuration window.":::

    1. Select **Shared Network Port-LOM** from the **Network Interface Adapter** field.

    1. Disable DHCP.

    1. Enter the IP address, subnet mask, and gateway IP address.

1. Select **F10: Save**.

1. Select **Esc** to get back to the **iLO 5 Configuration Utility**, and then select **User Management**.

1. Select **Edit/Remove User**. The administrator is the only default user defined.

1. Change the default password and select **F10: Save**.

### Configure the HPE BIOS

The following procedure describes how to configure the HPE BIOS for the enterprise and SMB appliances.

**To configure the HPE BIOS**:

1. Select **System Utilities** > **System Configuration** > **BIOS/Platform Configuration (RBSU)**.

1. In the **BIOS/Platform Configuration (RBSU)** form, select **Boot Options**.

1. Change **Boot Mode** to **Legacy BIOS Mode**, and then select **F10: Save**.

1. Select **Esc** twice to close the **System Configuration** form.

#### For the enterprise appliance

1. Select **Embedded RAID 1: HPE Smart Array P408i-a SR Gen 10** > **Array Configuration** > **Create Array**.

1. In the **Create Array** form, select all the options. Three options are available for the **Enterprise** appliance.

#### For the SMB appliance

1. Select **Embedded RAID 1: HPE Smart Array P208i-a SR Gen 10** > **Array Configuration** > **Create Array**.

1. Select **Proceed to Next Form**.

1. In the **Set RAID Level** form, set the level to **RAID 5** for enterprise deployments and **RAID 1** for SMB deployments.

1. Select **Proceed to Next Form**.

1. In the **Logical Drive Label** form, enter **Logical Drive 1**.

1. Select **Submit Changes**.

1. In the **Submit** form, select **Back to Main Menu**.

1. Select **F10: Save** and then press **Esc** twice.

1. In the **System Utilities** window, select **One-Time Boot Menu**.

1. In the **One-Time Boot Menu** form, select **Legacy BIOS One-Time Boot Menu**.

1. The **Booting in Legacy** and **Boot Override** windows appear. Choose a boot override option; for example, to a CD-ROM, USB, HDD, or UEFI shell.

    :::image type="content" source="media/tutorial-install-components/boot-override-window-one-v2.png" alt-text="Screenshot that shows the first Boot Override window.":::

    :::image type="content" source="media/tutorial-install-components/boot-override-window-two-v2.png" alt-text="Screenshot that shows the second Boot Override window.":::

### Software installation (HPE ProLiant DL20 appliance)

The installation process takes about 20 minutes. After the installation, the system is restarted several times.

To install the software:

1. Connect the screen and keyboard to the appliance, and then connect to the CLI.

1. Connect an external CD or disk on the key with the ISO image that you downloaded from the **Updates** page in the Defender for IoT portal.

1. Start the appliance.

1. Select **English**.

    :::image type="content" source="media/tutorial-install-components/select-english-screen.png" alt-text="Selection of English in the CLI window.":::

1. Select **SENSOR-RELEASE-\<version> Enterprise**.

    :::image type="content" source="media/tutorial-install-components/sensor-version-select-screen-v2.png" alt-text="Screenshot of the screen for selecting a version.":::

1. In the Installation Wizard define the hardware profile and network properties:

    :::image type="content" source="media/tutorial-install-components/installation-wizard-screen-v2.png" alt-text="Screenshot that shows the Installation Wizard.":::

    | Parameter | Configuration |
    | ----------| ------------- |
    | **Hardware profile** | Select **Enterprise** or **Office** for SMB deployments. |
    | **Management interface** | **eno2** |
    | **Default network parameters (usually the parameters are provided by the customer)** | **management network IP address:** <br/> <br/>**appliance hostname:** <br/>**DNS:** <br/>**the default gateway IP address:**|
    | **input interfaces:** | The system generates the list of input interfaces for you.<br/><br/>To mirror the input interfaces, copy all the items presented in the list with a comma separator: **eno5, eno3, eno1, eno6, eno4**<br/><br/>**For HPE DL20: Do not list eno1, enp1s0f4u4 (iLo interfaces)**<br/><br/>**BRIDGE**: There's no need to configure the bridge interface. This option is used for special use cases only. Press **Enter** to continue. |

1. After about 10 minutes, the two sets of credentials appear. One is for a **CyberX** user, and one is for a **Support** user.

1. Save the appliance's ID and passwords. You'll need the credentials to access the platform for the first time.

1. Select **Enter** to continue.

## HPE ProLiant DL360 installation

- A default administrative user is provided. We recommend that you change the password during the network configuration.

- During the network configuration, you'll configure the iLO port.

- The installation process takes about 20 minutes. After the installation, the system is restarted several times.

### HPE ProLiant DL360 front panel

:::image type="content" source="media/tutorial-install-components/hpe-proliant-dl360-front-panel.png" alt-text="HPE ProLiant DL360 front panel.":::

### HPE ProLiant DL360 back panel

:::image type="content" source="media/tutorial-install-components/hpe-proliant-dl360-back-panel.png" alt-text="HPE ProLiant DL360 back panel.":::

### Enable remote access and update the password

Refer to the preceding sections for HPE ProLiant DL20 installation:

- "Enable remote access and update the password"

- "Configure the HPE BIOS"

The enterprise configuration is identical.

> [!Note]
> In the array form, verify that you select all the options.

### iLO remote installation (from a virtual drive)

This procedure describes the iLO installation from a virtual drive.

To install:

1. Sign in to the iLO console, and then right-click the servers' screen.

1. Select **HTML5 Console**.

1. In the console, select the CD icon, and choose the CD/DVD option.

1. Select **Local ISO file**.

1. In the dialog box, choose the relevant ISO file.

1. Go to the left icon, select **Power**, and the select **Reset**.

1. The appliance will restart and run the sensor installation process.

### Software installation (HPE DL360)

The installation process takes about 20 minutes. After the installation, the system is restarted several times.

To install:

1. Connect the screen and keyboard to the appliance, and then connect to the CLI.

1. Connect an external CD or disk on a key with the ISO image that you downloaded from the **Updates** page in the Defender for IoT portal.

1. Start the appliance.

1. Select **English**.

1. Select **SENSOR-RELEASE-\<version> Enterprise**.

    :::image type="content" source="media/tutorial-install-components/sensor-version-select-screen-v2.png" alt-text="Screenshot that shows selecting the version.":::

1. In the Installation Wizard, define the appliance profile and network properties.

    :::image type="content" source="media/tutorial-install-components/installation-wizard-screen-v2.png" alt-text="Screenshot that shows the Installation Wizard.":::

    | Parameter | Configuration |
    | ----------| ------------- |
    | **Hardware profile** | Select **corporate**. |
    | **Management interface** | **eno2** |
    | **Default network parameters (provided by the customer)** | **management network IP address:** <br> **subnet mask:** <br/>**appliance hostname:** <br/>**DNS:** <br/>**the default gateway IP address:**|
    | **input interfaces:**  | The system generates a list of input interfaces for you.<br/><br/>To mirror the input interfaces, copy all the items presented in the list with a comma separator.<br/><br/> You do not need to configure the bridge interface. This option is used for special use cases only. |

1. After about 10 minutes, the two sets of credentials appear. One is for a **CyberX** user, and one is for a **support** user.

1. Save the appliance's ID and passwords. You'll need these credentials to access the platform for the first time.

1. Select **Enter** to continue.

## HP EdgeLine 300 installation

- A default administrative user is provided. We recommend that you change the password during the network configuration.

- The installation process takes about 20 minutes. After the installation, the system is restarted several times.

### HP EdgeLine 300 back panel

:::image type="content" source="media/tutorial-install-components/edgeline-el300-panel.png" alt-text="View of the back panel of the EL300":::

### Enable remote access

1. Enter the iSM IP Address into your web browser.

1. Sign in using the default username and password found on your appliance.

1. Navigate to **Wired and Wireless Network** > **IPV4**

    :::image type="content" source="media/tutorial-install-components/wired-and-wireless.png" alt-text="navigate to highlighted sections.":::

1. Disable **DHCP toggle**.

1. Configure the IPv4 addresses as such:
    - **IPV4 Address**: `192.168.1.125`
    - **IPV4 Subnet Mask**: `255.255.255.0`
    - **IPV4 Gateway**: `192.168.1.1`

1. Select **Apply**.

1. Sign out and reboot the appliance.

### Configure the BIOS

The following procedure describes how to configure the BIOS for HP EL300 appliance.

**To configure the BIOS**:

1. Turn on the appliance and push **F9** to enter the BIOS.

1. Select **Advanced**, and scroll down to **CSM Support**.

    :::image type="content" source="media/tutorial-install-components/csm-support.png" alt-text="Enable CSM support to open the additional menu.":::

1. Push **Enter** to enable CSM Support.

1. Navigate to **Storage** and push **+/-** to change it to Legacy.

1. Navigate to **Video** and push **+/-** to change it to Legacy.

    :::image type="content" source="media/tutorial-install-components/storage-and-video.png" alt-text="Navigate to storage and video and change them to Legacy.":::

1. Navigate to **Boot** > **Boot mode select**.

1. Push **+/-** to change it to Legacy.

    :::image type="content" source="media/tutorial-install-components/boot-mode.png" alt-text="Change Boot mode select to Legacy.":::

1. Navigate to **Save & Exit**.

1. Select **Save Changes and Exit**.

    :::image type="content" source="media/tutorial-install-components/save-and-exit.png" alt-text="Save your changes and exit the system.":::

1. Select **Yes**, and the appliance will reboot.

1. Push **F11** to enter the **Boot Menu**.

1. Select the device with the sensor image. Either **DVD** or **USB**.

1. Select your language.

1. Select **sensor-10.0.3.12-62a2a3f724 Office: 4 CPUS, 8GB RAM, 100GB STORAGE**.

    :::image type="content" source="media/tutorial-install-components/sensor-select-screen.png" alt-text="Select the sensor version as shown.":::

1. In the Installation Wizard, define the appliance profile, and network properties:

    :::image type="content" source="media/tutorial-install-components/appliance-parameters.png" alt-text="Define the appliance's profile and network configurations with the following parameters.":::

    | Parameter | Configuration |
    |--|--|
    | **configure hardware profile** | **office** |
    | **configure management network interface** | **enp3s0** <br /> or <br />**possible value** |
    | **configure management network IP address:** | **IP address provided by the customer** |
    | **configure subnet mask:** | **IP address provided by the customer** |
    | **configure DNS:** | **IP address provided by the customer** |
    | **configure default gateway IP address:** | **IP address provided by the customer** |
    | **configure input interface(s)** | **enp4s0** <br /> or <br />**possible value** |
    | **configure bridge interface(s)** | N/A |

1. Accept the settings and continue by entering `Y`.

## Sensor installation for the virtual appliance

You can deploy the virtual machine for the Defender for IoT sensor in the following architectures:

| Architecture | Specifications | Usage | Comments |
|---|---|---|---|
| **Enterprise** | CPU: 8<br/>Memory: 32G RAM<br/>HDD: 1800 GB | Production environment | Default and most common |
| **Small Business** | CPU: 4 <br/>Memory: 8G RAM<br/>HDD: 500 GB | Test or small production environments | -  |
| **Office** | CPU: 4<br/>Memory: 8G RAM<br/>HDD: 100 GB | Small test environments | -  |

### Prerequisites

The on-premises management console supports both VMware and Hyper-V deployment options. Before you begin the installation, make sure you have the following items:

- VMware (ESXi 5.5 or later) or Hyper-V hypervisor (Windows 10 Pro or Enterprise) installed and operational

- Available hardware resources for the virtual machine

- ISO installation file for the Azure Defender for IoT sensor

Make sure the hypervisor is running.

### Create the virtual machine (ESXi)

1. Sign in to the ESXi, choose the relevant **datastore**, and select **Datastore Browser**.

1. **Upload** the image and select **Close**.

1. Go to **Virtual Machines**, and then select **Create/Register VM**.

1. Select **Create new virtual machine**, and then select **Next**.

1. Add a sensor name and choose:

   - Compatibility: **&lt;latest ESXi version&gt;**

   - Guest OS family: **Linux**

   - Guest OS version: **Ubuntu Linux (64-bit)**

1. Select **Next**.

1. Choose the relevant datastore and select **Next**.

1. Change the virtual hardware parameters according to the required architecture.

1. For **CD/DVD Drive 1**, select **Datastore ISO file** and choose the ISO file that you uploaded earlier.

1. Select **Next** > **Finish**.

### Create the virtual machine (Hyper-V)

This procedure describes how to create a virtual machine by using Hyper-V.

To create a virtual machine:

1. Create a virtual disk in Hyper-V Manager.

1. Select **format = VHDX**.

1. Select **type = Dynamic Expanding**.

1. Enter the name and location for the VHD.

1. Enter the required size (according to the architecture).

1. Review the summary and select **Finish**.

1. On the **Actions** menu, create a new virtual machine.

1. Enter a name for the virtual machine.

1. Select **Specify Generation** > **Generation 1**.

1. Specify the memory allocation (according to the architecture) and select the check box for dynamic memory.

1. Configure the network adaptor according to your server network topology.

1. Connect the VHDX created previously to the virtual machine.

1. Review the summary and select **Finish**.

1. Right-click the new virtual machine and select **Settings**.

1. Select **Add Hardware** and add a new network adapter.

1. Select the virtual switch that will connect to the sensor management network.

1. Allocate CPU resources (according to the architecture).

1. Connect the management console's ISO image to a virtual DVD drive.

1. Start the virtual machine.

1. On the **Actions** menu, select **Connect** to continue the software installation.

### Software installation (ESXi and Hyper-V)

This section describes the ESXi and Hyper-V software installation.

To install:

1. Open the virtual machine console.

1. The VM will start from the ISO image, and the language selection screen will appear. Select **English**.

1. Select the required architecture.

1. Define the appliance profile and network properties:

    | Parameter | Configuration |
    | ----------| ------------- |
    | **Hardware profile** | &lt;required architecture&gt; |
    | **Management interface** | **ens192** |
    | **Network parameters (provided by the customer)** | **management network IP address:** <br/>**subnet mask:** <br/>**appliance hostname:** <br/>**DNS:** <br/>**default gateway:** <br/>**input interfaces:**|
    | **bridge interfaces:** | There's no need to configure the bridge interface. This option is for special use cases only. |

1. Enter **Y** to accept the settings.

1. Sign-in credentials are automatically generated and presented. Copy the username and password in a safe place, because they're required for sign-in and administration.

      - **Support**: The administrative user for user management.

      - **CyberX**: The equivalent of root for accessing the appliance.

1. The appliance restarts.

1. Access the management console via the IP address previously configured: `https://ip_address`.

    :::image type="content" source="media/tutorial-install-components/defender-for-iot-sign-in-screen.png" alt-text="Screenshot that shows access to the management console.":::

## On-premises management console installation

Before installing the software on the appliance, you need to adjust the appliance's BIOS configuration:

### BIOS configuration

**To configure the BIOS for your appliance**:

1. [Enable remote access and update the password](#enable-remote-access-and-update-the-password).

1. [Configure the BIOS](#configure-the-hpe-bios).

### Software installation

The installation process takes about 20 minutes. After the installation, the system is restarted several times.

During the installation process, you will can add a secondary NIC. If you choose not to install the secondary NIC during installation, you can [add a secondary NIC](#add-a-secondary-nic) at a later time.

To install the software:

1. Select your preferred language for the installation process.

   :::image type="content" source="media/tutorial-install-components/on-prem-language-select.png" alt-text="Select your preferred language for the installation process.":::

1. Select **MANAGEMENT-RELEASE-\<version\>\<deployment type\>**.

   :::image type="content" source="media/tutorial-install-components/on-prem-install-screen.png" alt-text="Select your version.":::

1. In the Installation Wizard, define the network properties:

   :::image type="content" source="media/tutorial-install-components/on-prem-first-steps-install.png" alt-text="Screenshot that shows the appliance profile.":::

   | Parameter | Configuration |
   |--|--|
   | **configure management network interface** | For Dell: **eth0, eth1** <br /> For HP: **enu1, enu2** <br>  or <br />**possible value** |
   | **configure management network IP address:** | **IP address provided by the customer** |
   | **configure subnet mask:** | **IP address provided by the customer** |
   | **configure DNS:** | **IP address provided by the customer** |
   | **configure default gateway IP address:** | **IP address provided by the customer** |

1. **(Optional)** If you would like to install a secondary Network Interface Card (NIC), define the following appliance profile, and network properties:

    :::image type="content" source="media/tutorial-install-components/on-prem-secondary-nic-install.png" alt-text="Screenshot that shows the Secondary NIC install questions.":::

   | Parameter | Configuration |
   |--|--|
   | **configure sensor monitoring interface (Optional):** | **eth1**, or **possible value** |
   | **configure an IP address for the sensor monitoring interface:** | **IP address provided by the customer** |
   | **configure a subnet mask for the sensor monitoring interface:** | **IP address provided by the customer** |

1. Accept the settlings and continue by typing `Y`.

1. After about 10 minutes, the two sets of credentials appear. One is for a **CyberX** user, and one is for a **Support** user.

   :::image type="content" source="media/tutorial-install-components/credentials-screen.png" alt-text="Copy these credentials as they will not be presented again.":::  

   Save the usernames, and passwords, you'll need these credentials to access the platform the first time you use it.

1. Select **Enter** to continue.

For information on how to find the physical port on your appliance, see [Find your port](#find-your-port).

### Add a secondary NIC

You can enhance security to your on-premises management console by adding a secondary NIC. By adding a secondary NIC you will have one dedicated for your users, and the other will support the configuration of a gateway for routed networks. The second NIC is dedicated to all attached sensors within an IP address range.

:::image type="content" source="media/tutorial-install-components/secondary-nic.png" alt-text="The overall architecture of the secondary NIC.":::

Both NICs will support the user interface (UI).
If you choose not to deploy a secondary NIC, all of the features will be available through the primary NIC.

If you have already configured your on-premises management console, and would like to add a secondary NIC to your on-premises management console, use the following steps:

1. Use the network reconfigure command:

    ```bash
    sudo cyberx-management-network-reconfigure
    ```

1. Enter the following responses to the following questions:

    :::image type="content" source="media/tutorial-install-components/network-reconfig-command.png" alt-text="Enter the following answers to configure your appliance.":::

    | Parameters | Response to enter |
    |--|--|
    | **Management Network IP address** | `N` |
    | **Subnet mask** | `N` |
    | **DNS** | `N` |
    | **Default gateway IP Address** | `N` |
    | **Sensor monitoring interface (Optional. Applicable when sensors are on a different network segment. For more information, see the Installation instructions)**| `Y`, **select a possible value** |
    | **An IP address for the sensor monitoring interface (accessible by the sensors)** | `Y`, **IP address provided by the customer**|
    | **A subnet mask for the sensor monitoring interface (accessible by the sensors)** | `Y`, **IP address provided by the customer** |
    | **Hostname** | **provided by the customer** |

1. Review all choices, and enter `Y` to accept the changes. The system reboots.

### Find your port

If you are having trouble locating the physical port on your device, you can use the following command to:

```bash
sudo ethtool -p <port value> <time-in-seconds>
```

This command will cause the light on the port to flash for the specified time period. For example, entering `sudo ethtool -p eno1 120`, will have port eno1 flash for 2 minutes allowing you to find the port on the back of your appliance.

## Virtual appliance: On-premises management console installation

The on-premises management console VM supports the following architectures:

| Architecture | Specifications | Usage |
|--|--|--|
| Enterprise <br/>(Default and most common) | CPU: 8 <br/>Memory: 32G RAM<br/> HDD: 1.8 TB | Large production environments |
| Small | CPU: 4 <br/> Memory: 8G RAM<br/> HDD: 500 GB | Large production environments |
| Office | CPU: 4 <br/>Memory: 8G RAM <br/> HDD: 100 GB | Small test environments |

### Prerequisites

The on-premises management console supports both VMware and Hyper-V deployment options. Before you begin the installation, verify the following:

- VMware (ESXi 5.5 or later) or Hyper-V hypervisor (Windows 10 Pro or Enterprise) is installed and operational.

- The hardware resources are available for the virtual machine.

- You have the ISO installation file for the on-premises management console.

- The hypervisor is running.

### Create the virtual machine (ESXi)

To a create virtual machine (ESXi):

1. Sign in to the ESXi, choose the relevant **datastore**, and select **Datastore Browser**.

1. Upload the image and select **Close**.

1. Go to **Virtual Machines**.

1. Select **Create/Register VM**.

1. Select **Create new virtual machine** and select **Next**.

1. Add a sensor name and choose:

   - Compatibility: \<latest ESXi version>

   - Guest OS family: Linux

   - Guest OS version: Ubuntu Linux (64-bit)

1. Select **Next**.

1. Choose relevant datastore and select **Next**.

1. Change the virtual hardware parameters according to the required architecture.

1. For **CD/DVD Drive 1**, select **Datastore ISO file** and choose the ISO file that you uploaded earlier.

1. Select **Next** > **Finish**.

### Create the virtual machine (Hyper-V)

To create a virtual machine by using Hyper-V:

1. Create a virtual disk in Hyper-V Manager.

1. Select the format **VHDX**.

1. Select **Next**.

1. Select the type **Dynamic expanding**.

1. Select **Next**.

1. Enter the name and location for the VHD.

1. Select **Next**.

1. Enter the required size (according to the architecture).

1. Select **Next**.

1. Review the summary and select **Finish**.

1. On the **Actions** menu, create a new virtual machine.

1. Select **Next**.

1. Enter a name for the virtual machine.

1. Select **Next**.

1. Select **Generation** and set it to **Generation 1**.

1. Select **Next**.

1. Specify the memory allocation (according to the architecture) and select the check box for dynamic memory.

1. Select **Next**.

1. Configure the network adaptor according to your server network topology.

1. Select **Next**.

1. Connect the VHDX created previously to the virtual machine.

1. Select **Next**.

1. Review the summary and select **Finish**.

1. Right-click the new virtual machine, and then select **Settings**.

1. Select **Add Hardware** and add a new adapter for **Network Adapter**.

1. For **Virtual Switch**, select the switch that will connect to the sensor management network.

1. Allocate CPU resources (according to the architecture).

1. Connect the management console's ISO image to a virtual DVD drive.

1. Start the virtual machine.

1. On the **Actions** menu, select **Connect** to continue the software installation.

### Software installation (ESXi and Hyper-V)

Starting the virtual machine will start the installation process from the ISO image.

To install the software:

1. Select **English**.

1. Select the required architecture for your deployment.

1. Define the network interface for the sensor management network: interface, IP, subnet, DNS server, and default gateway.

1. Sign-in credentials are automatically generated. Save the username and passwords, you'll need these credentials to access the platform the first time you use it.

   The appliance will then reboot.

1. Access the management console via the IP address previously configured: `<https://ip_address>`.

    :::image type="content" source="media/tutorial-install-components/defender-for-iot-management-console-sign-in-screen.png" alt-text="Screenshot that shows the management console's sign-in screen.":::

## Legacy appliances

This section describes installation procedures for *legacy* appliances only. See [Identify required appliances](how-to-identify-required-appliances.md#identify-required-appliances), if you are buying a new appliance.

### Nuvo 5006LP installation

This section provides the Nuvo 5006LP installation procedure. Before installing the software on the Nuvo 5006LP appliance, you need to adjust the appliance BIOS configuration.

#### Nuvo 5006LP front panel

:::image type="content" source="media/tutorial-install-components/nuvo5006lp_frontpanel.png" alt-text="A view of the front panel of the Nuvo 5006LP device.":::

1. Power button, Power indicator
1. DVI video connectors
1. HDMI video connectors
1. VGA video connectors
1. Remote on/off Control, and status LED output
1. Reset button
1. Management network adapter
1. Ports to receive mirrored data

#### Nuvo back panel

:::image type="content" source="media/tutorial-install-components/nuvo5006lp_backpanel.png" alt-text="A view of the back panel of the Nuvo 5006lp.":::

1. SIM card slot
1. Microphone, and speakers
1. COM ports
1. USB connectors
1. DC power port (DC IN)

#### Configure the Nuvo 5006LP BIOS

The following procedure describes how to configure the Nuvo 5006LP BIOS. Make sure the operating system was previously installed on the appliance.

**To configure the BIOS**:

1. Power on the appliance.

1. Press **F2** to enter the BIOS configuration.

1. Navigate to **Power** and change Power On after Power Failure to S0-Power On.

    :::image type="content" source="media/tutorial-install-components/nuvo-power-on.png" alt-text="Change you Nuvo 5006 to power on after a power failure..":::

1. Navigate to **Boot** and ensure that **PXE Boot to LAN** is set to **Disabled**.

1. Press **F10** to save, and then select **Exit**.

#### Software installation (Nuvo 5006LP)

The installation process takes approximately 20 minutes. After installation, the system is restarted several times.

**To install the software**:

1. Connect the external CD, or disk on key with the ISO image.

1. Boot the appliance.

1. Select **English**.

1. Select **XSENSE-RELEASE-\<version> Office...**.

    :::image type="content" source="media/tutorial-install-components/sensor-version-select-screen-v2.png" alt-text="Select the version of the sensor to install.":::

1. Define the appliance architecture, and network properties:

    :::image type="content" source="media/tutorial-install-components/nuvo-profile-appliance.png" alt-text="Define the Nuvo's architecture and network properties.":::

    | Parameter | Configuration |
    | ----------| ------------- |
    | **Hardware profile** | Select **office**. |
    | **Management interface** | **eth0** |
    | **Management network IP address** | **IP address provided by the customer** |
    | **Management subnet mask** | **IP address provided by the customer** |
    | **DNS** | **IP address provided by the customer** |
    | **Default gateway IP address** | **0.0.0.0** |
    | **Input interface** | The list of input interfaces is generated for you by the system. <br />To mirror the input interfaces, copy all the items presented in the list with a comma separator. |
    | **Bridge interface** | - |

1. Accept the settings and continue by entering `Y`.

After approximately 10 minutes, sign-in credentials are automatically generated. Save the username and passwords, you'll need these credentials to access the platform the first time you use it.

### Fitlet2 mini sensor Installation

This section provides the Fitlet2 installation procedure. Before installing the software on the Fitlet appliance, you need to adjust the appliance's BIOS configuration.

#### Fitlet2 front panel

:::image type="content" source="media/tutorial-install-components/fitlet-front-panel.png" alt-text="A view of the front panel of the Fitlet 2.":::

#### Fitlet2 back panel

:::image type="content" source="media/tutorial-install-components/fitlet2-back-panel.png" alt-text="A view of the back panel of the Fitlet 2.":::

#### Configure the Fitlet2 BIOS

**To configure the Fitlet2 BIOS**:

1. Power on the appliance.

1. Navigate to **Main** > **OS Selection**.

1. Press **+/-** to select **Linux**.

    :::image type="content" source="media/tutorial-install-components/fitlet-linux.png" alt-text="Set the OS to Linux on your Fitlet2.":::

1. Verify that the system date, and time are updated with the installation date, and time.

1. Navigate to **Advanced**, and select **ACPI Settings**.

1. Select **Enable Hibernation**, and press **+/-** to select **Disabled**.

    :::image type="content" source="media/tutorial-install-components/disable-hibernation.png" alt-text="Diable the hibernation mode on your Fitlet2.":::

1. Press **Esc**.

1. Navigate to **Advanced** > **TPM Configuration**.

1. Select **fTPM**, and press **+/-** to select **Disabled**.

1. Press **Esc**.

1. Navigate to **CPU Configuration** > **VT-d**.

1. Press **+/-** to select **Enabled**.

1. Navigate to **CSM Configuration** > **CSM Support**.

1. Press **+/-** to select **Enabled**.

1. Navigate to **Advanced** > **Boot option filter [Legacy only]** and change setting in the following fields to **Legacy**:

    - Network
    - Storage
    - Video
    - Other PCI

    :::image type="content" source="media/tutorial-install-components/legacy-only.png" alt-text="Set all fields to Legacy.":::

1. Press **Esc**.

1. Navigate to **Security** > **Secure Boot Customization**.

1. Press **+/-** to select **Disabled**.

1. Press **Esc**.

1. Navigate to **Boot** > **Boot mode** select, and select **Legacy**.

1. Select **Boot Option #1 – [USB CD/DVD]**.

1. Select **Save & Exit**.

#### Software installation (Fitlet2)

The installation process takes approximately 20 minutes. After installation, the system is restarted several times.

1. Connect the external CD, or disk on key with the ISO image.

1. Boot the appliance.

1. Select **English**.

1. Select **XSENSE-RELEASE-\<version> Office...**.

    :::image type="content" source="media/tutorial-install-components/sensor-version-select-screen-v2.png" alt-text="Select the version of the sensor to install.":::

    > [!Note]
    > Do not select Ruggedized.

1. Define the appliance architecture, and network properties:

    :::image type="content" source="media/tutorial-install-components/nuvo-profile-appliance.png" alt-text="Define the Nuvo's architecture and network properties.":::

    | Parameter | Configuration |
    | ----------| ------------- |
    | **Hardware profile** | Select **office**. |
    | **Management interface** | **em1** |
    | **Management network IP address** | **IP address provided by the customer** |
    | **Management subnet mask** | **IP address provided by the customer** |
    | **DNS** | **IP address provided by the customer** |
    | **Default gateway IP address** | **0.0.0.0** |
    | **Input interface** | The list of input interfaces is generated for you by the system. <br />To mirror the input interfaces, copy all the items presented in the list with a comma separator. |
    | **Bridge interface** | - |

1. Accept the settings and continue by entering `Y`.

After approximately 10 minutes, sign-in credentials are automatically generated. Save the username and passwords, you'll need these credentials to access the platform the first time you use it.

## Post-installation validation

To validate the installation of a physical appliance, you need to perform many tests. The same validation process applies to all the appliance types.

Perform the validation by using the GUI or the CLI. The validation is available to the user **Support** and the user **CyberX**.

Post-installation validation must include the following tests:

- **Sanity test**: Verify that the system is running.

- **Version**: Verify that the version is correct.

- **ifconfig**: Verify that all the input interfaces configured during the installation process are running.

### Check system health by using the GUI

:::image type="content" source="media/tutorial-install-components/system-health-check-screen.png" alt-text="Screenshot that shows the system health check.":::

#### Sanity

- **Appliance**: Runs the appliance sanity check. You can perform the same check by using the CLI command `system-sanity`.

- **Version**: Displays the appliance version.

- **Network Properties**: Displays the sensor network parameters.

#### Redis

- **Memory**: Provides the overall picture of memory usage, such as how much memory was used and how much remained.

- **Longest Key**: Displays the longest keys that might cause extensive memory usage.

#### System

- **Core Log**: Provides the last 500 rows of the core log, enabling you to view the recent log rows without exporting the entire system log.

- **Task Manager**: Translates the tasks that appear in the table of processes to the following layers:
  
  - Persistent layer (Redis)

  - Cash layer (SQL)

- **Network Statistics**: Displays your network statistics.

- **TOP**: Shows the table of processes. It's a Linux command that provides a dynamic real-time view of the running system.

- **Backup Memory Check**: Provides the status of the backup memory, checking the following:

  - The location of the backup folder

  - The size of the backup folder

  - The limitations of the backup folder

  - When the last backup happened

  - How much space there is for the extra backup files

- **ifconfig**: Displays the parameters for the appliance's physical interfaces.

- **CyberX nload**: Displays network traffic and bandwidth by using the six-second tests.

- **Errors from Core, log**: Displays errors from the core log file.

**To access the tool**:

1. Sign in to the sensor with the **Support** user credentials.

1. Select **System Statistics** from the **System Settings** window.

    :::image type="icon" source="media/tutorial-install-components/system-statistics-icon.png" border="false":::

### Check system health by using the CLI

Verify that the system is up, and running prior to testing the system's sanity.

**To test the system's sanity**:

1. Connect to the CLI with the Linux terminal (for example, PuTTY) and the user **Support**.

1. Enter `system sanity`.

1. Check that all the services are green (running).

    :::image type="content" source="media/tutorial-install-components/support-screen.png" alt-text="Screenshot that shows running services.":::

1. Verify that **System is UP! (prod)** appears at the bottom.

Verify that the correct version is used:

**To check the system's version**:

1. Connect to the CLI with the Linux terminal (for example, PuTTY) and the user **Support**.

1. Enter `system version`.

1. Check that the correct version appears.

Verify that all the input interfaces configured during the installation process are running:

**To validate the system's network status**:

1. Connect to the CLI with the Linux terminal (for example, PuTTY) and the user **Support**.

1. Enter `network list` (the equivalent of the Linux command `ifconfig`).

1. Validate that the required input interfaces appear. For example, if two quad Copper NICs are installed, there should be 10 interfaces in the list.

    :::image type="content" source="media/tutorial-install-components/interface-list-screen.png" alt-text="Screenshot that shows the list of interfaces.":::

Verify that you can access the console web GUI:

**To check that management has access to the UI**:

1. Connect a laptop with an Ethernet cable to the management port (**Gb1**).

1. Define the laptop NIC address to be in the same range as the appliance.

    :::image type="content" source="media/tutorial-install-components/access-to-ui.png" alt-text="Screenshot that shows management access to the UI.":::

1. Ping the appliance's IP address from the laptop to verify connectivity (default: 10.100.10.1).

1. Open the Chrome browser in the laptop and enter the appliance's IP address.

1. In the **Your connection is not private** window, select **Advanced** and proceed.

1. The test is successful when the Defender for IoT sign-in screen appears.

   :::image type="content" source="media/tutorial-install-components/defender-for-iot-sign-in-screen.png" alt-text="Screenshot that shows access to management console.":::

## Troubleshooting

### You can't connect by using a web interface

1. Verify that the computer that you're trying to connect is on the same network as the appliance.

1. Verify that the GUI network is connected to the management port.

1. Ping the appliance's IP address. If there is no ping:

   1. Connect a monitor and a keyboard to the appliance.

   1. Use the **Support** user and password to sign in.

   1. Use the command `network list` to see the current IP address.

      :::image type="content" source="media/tutorial-install-components/network-list.png" alt-text="Screenshot that shows the network list.":::

1. If the network parameters are misconfigured, use the following procedure to change them:

   1. Use the command `network edit-settings`.

   1. To change the management network IP address, select **Y**.

   1. To change the subnet mask, select **Y**.

   1. To change the DNS, select **Y**.

   1. To change the default gateway IP address, select **Y**.

   1. For the input interface change (sensor only), select **N**.

   1. To apply the settings, select **Y**.

1. After restart, connect with the support user credentials and use the `network list` command to verify that the parameters were changed.

1. Try to ping and connect from the GUI again.

### The appliance isn't responding

1. Connect a monitor and keyboard to the appliance, or use PuTTY to connect remotely to the CLI.

1. Use the **Support** user's credentials to sign in.

1. Use the `system sanity` command and check that all processes are running.

    :::image type="content" source="media/tutorial-install-components/system-sanity-screen.png" alt-text="Screenshot that shows the system sanity command.":::

For any other issues, contact [Microsoft Support](https://support.microsoft.com/en-us/supportforbusiness/productselection?sapId=82c88f35-1b8e-f274-ec11-c6efdd6dd099).

## Configure a SPAN port

A virtual switch does not have mirroring capabilities. However, you can use promiscuous mode in a virtual switch environment. Promiscuous mode  is a mode of operation, as well as a security, monitoring and administration technique, that is defined at the virtual switch, or portgroup level. By default, Promiscuous mode is disabled. When Promiscuous mode is enabled the virtual machine’s network interfaces that are in the same portgroup will use the Promiscuous mode to view all network traffic that goes through that virtual switch. You can implement a workaround with either ESXi, or Hyper-V.

:::image type="content" source="media/tutorial-install-components/purdue-model.png" alt-text="A screenshot of where in your architecture the sensor should be placed.":::

### Configure a SPAN port with ESXi

**To configure a SPAN port with ESXi**:

1. Open vSwitch properties.

1. Select **Add**.

1. Select **Virtual Machine** > **Next**.

1. Insert a network label **SPAN Network**, select **VLAN ID** > **All**, and then select **Next**.

1. Select **Finish**.

1. Select **SPAN Network** > **Edit*.

1. Select **Security**, and verify that the **Promiscuous Mode** policy is set to **Accept** mode.

1. Select **OK**, and then select **Close** to close the vSwitch properties.

1. Open the **XSense VM** properties.

1. For **Network Adapter 2**, select the **SPAN** network.

1. Select **OK**.

1. Connect to the sensor, and verify that mirroring works.

### Configure a SPAN port with Hyper-V

Prior to starting you will need to:

- Ensure that there is no instance of a virtual appliance running.

- Enable Ensure SPAN on the data port, and not the management port.

- Ensure that the data port SPAN configuration is not configured with an IP address.

**To configure a SPAN port with Hyper-V**:

1. Open the Virtual Switch Manager.

1. In the Virtual Switches list, select **New virtual network switch** > **External** as the dedicated spanned network adapter type.

    :::image type="content" source="media/tutorial-install-components/new-virtual-network.png" alt-text="Screenshot of selecting new virtual network and external before creating the virtual switch.":::

1. Select **Create Virtual Switch**.

1. Under connection type, select **External Network**.

1. Ensure the checkbox for **Allow management operating system to share this network adapter** is checked.

   :::image type="content" source="media/tutorial-install-components/external-network.png" alt-text="Select external network, and allow the management operating system to share the network adapter.":::

1. Select **OK**.

#### Attach a SPAN Virtual Interface to the virtual switch

You are able to attach a SPAN Virtual Interface to the Virtual Switch through Windows PowerShell, or through Hyper-V Manager.

**To attach a SPAN Virtual Interface to the virtual switch with PowerShell**:

1. Select the newly added SPAN virtual switch, and add a new network adapter with the following command:

    ```bash
    ADD-VMNetworkAdapter -VMName VK-C1000V-LongRunning-650 -Name Monitor -SwitchName vSwitch_Span
    ```

1. Enable port mirroring for the selected interface as the span destination with the following command:

    ```bash
    Get-VMNetworkAdapter -VMName VK-C1000V-LongRunning-650 | ? Name -eq Monitor | Set-VMNetworkAdapter -PortMirroring Destination
    ```

    | Parameter | Description |
    |--|--|
    | VK-C1000V-LongRunning-650 | CPPM VA name |
    |vSwitch_Span |Newly added SPAN virtual switch name |
    |Monitor |Newly added adapter name |

1. Select **OK**.

These commands set the name of the newly added adapter hardware to be `Monitor`. If you are using Hyper-V Manager, the name of the newly added adapter hardware is set to `Network Adapter`.

**To attach a SPAN Virtual Interface to the virtual switch with Hyper-V Manager**:

1. Under the Hardware list, select **Network Adapter**.

1. In the Virtual Switch field, select **vSwitch_Span**.

    :::image type="content" source="media/tutorial-install-components/vswitch-span.png" alt-text="Screenshot of selecting the following options on the virtual switch screen.":::

1. In the Hardware list, under the Network Adapter drop-down list, select **Advanced Features**.

1. In the Port Mirroring section, select **Destination** as the mirroring mode for the new virtual interface.

    :::image type="content" source="media/tutorial-install-components/destination.png" alt-text="Screenshot of the selections needed to configure mirroring mode.":::

1. Select **OK**.

#### Enable Microsoft NDIS capture extensions for the virtual switch

Microsoft NDIS Capture Extensions will need to be enabled for the new virtual switch.

**To enable Microsoft NDIS capture extensions for the newly added virtual switch**:

1. Open the Virtual Switch Manager on the Hyper-V host.

1. In the Virtual Switches list, expand the virtual switch name `vSwitch_Span` and select **Extensions**.

1. In the Switch Extensions field, select **Microsoft NDIS Capture**.

    :::image type="content" source="media/tutorial-install-components/microsoft-ndis.png" alt-text="Screenshot of enabling the Microsoft NDIS by selecting it from the switch extensions menu.":::

1. Select **OK**.

#### Set the Mirroring Mode on the external port

Mirroring mode will need to be set on the external port of the new virtual switch to be the source.

You will need to configure the Hyper-V virtual switch (vSwitch_Span) to forward any traffic that comes to the external source port, to the virtual network adapter that you configured as the destination.

Use the following PowerShell commands to set the external virtual switch port to source mirror mode:

```bash
$ExtPortFeature=Get-VMSystemSwitchExtensionPortFeature -FeatureName "Ethernet Switch Port Security Settings"
$ExtPortFeature.SettingData.MonitorMode=2
Add-VMSwitchExtensionPortFeature -ExternalPort -SwitchName vSwitch_Span -VMSwitchExtensionFeature $ExtPortFeature
```

| Parameter | Description |
|--|--|
| vSwitch_Span | Newly added SPAN virtual switch name. |
| MonitorMode=2 | Source |
| MonitorMode=1 | Destination |
| MonitorMode=0 | None |

Use the following PowerShell command to verify the monitoring mode status:

```bash
Get-VMSwitchExtensionPortFeature -FeatureName "Ethernet Switch Port Security Settings" -SwitchName vSwitch_Span -ExternalPort | select -ExpandProperty SettingData
```

| Parameter | Description |
|--|--|
| vSwitch_Span | Newly added SPAN virtual switch name |

## Access sensors from the on-premises management console

You can enhance system security by preventing direct user access to the sensor. Instead, use proxy tunneling to let users access the sensor from the on-premises management console with a single firewall rule. This technique narrows the possibility of unauthorized access to the network environment beyond the sensor. The user's experience when signing in to the sensor remains the same.

:::image type="content" source="media/tutorial-install-components/sensor-system-graph.png" alt-text="Screenshot that shows access to the sensor.":::

**To enable tunneling**:

1. Sign in to the on-premises management console's CLI with the **CyberX**, or the **Support** user credentials.

1. Enter `sudo cyberx-management-tunnel-enable`.

1. Select **Enter**.

1. Enter `--port 10000`.

### Next steps

[Set up your network](how-to-set-up-your-network.md)
