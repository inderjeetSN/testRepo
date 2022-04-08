# Ignite IT service productivity with Client Software Distribution 2.0


## Lab 1:

### Overview

Client Software Distribution 2.0 application (CSD 2.0) application enables administrators to distribute software from the service catalog using third-party management systems.
CSD 2.0 enables an administrator to create all the records necessary to deploy software from service catalog requests, including software models and catalog items.

You can use the CSD 2.0 application to automate the deployment and revocation of software from a third-party software provider using the custom flows. Deployment and revocation is accomplished using provider-specific spoke flows, sub-flows, and actions. 

This lab guide will walk you through the process of using JAMF software provider through CSD 2.0.


### Installation:

1.	Navigate to Plugins from navigator.
2.	List of  all available applications will be displayed.
3.	Search for “Client Software Distribution 2.0” and install it. Perform same action for “Jamf Spoke”.

<img src="./media/image1.png"
style="width:6.26389in;height:1.89792in" border="10"/>

<img src="./media/image2.png"
style="width:6.26389in;height:1.89792in" />

A successful installation displays following result:

<img src="./media/image3.png"
style="width:6.26389in;height:1.20457in" />

**CSD 2.0 Modules**

<img src="./media/image4.png"
style="width:6.26389in;height:1.95486in" />

**Jamf Modules:**

<img src="./media/image5.png"
style="width:6.26389in;height:1.80556in" />


### CSD 2.0 - JAMF Integration:

Use the CSD 2.0 along with the Jamf spoke to deploy and revoke software
deployments from Jamf  server and manage distributions on hosts.

The Jamf spoke contains actions that CSD 2.0 uses to deploy software
from a service catalog request and manage user and device groups
on Jamf servers. In addition, CSD 2.0 can manage license counts for
deployed software, revoke software deployed by Jamf without user
interaction, and manage lease periods.

### Configuration Check:

**Verify Provider record:**

Verify that the flows are configured correctly in the provider record.

1.  Navigate to **Client Software Distribution 2.0** \> **Providers**.

2.  Open the record **Jamf**.

    <img src="./media/image6.png" style="width:5.9152in;height:1.53979in" />

3.  Verify that the flows are configured correctly in the record.

    <img src="./media/image7.png" style="width:6.26389in;height:1.77708in" />

    **Note:** You can configure the default flows as per your requirement.



**Deciding license tables:**

CSD 2.0 enables you to use its own license tables or you can also
consume license information from SAM (Software Asset Management) tables.

Use the first property shown below to make your choice.

<img src="./media/image8.png" style="width:6.26389in;height:2.79792in" />

   **Note:** Edit other properties as per requirement.

Software Asset Management (**SAM**) tables:

-   Software Entitlement (alm_license)

-   Software Model (cmdb_software_product_model)

Client Software Distribution 2.0 (**CSD 2.0**) tables:

-   Software Entitlement (sn_csd_license)

-   Software Model (sn_csd_software_product_model)
  
  
## Lab 2:

### Setting up JAMF Spoke:

Integrate the ServiceNow instance and Jamf instance using
the Jamf credential to authenticate ServiceNow requests.

**Role required:** admin

**Procedure:**

1.  Navigate to **Connections & Credentials** \> **Connection &
    Credential Aliases**.

2.  Open the record, **Jamf**.

    <img src="./media/image9.png" style="width:6.26389in;height:1.55811in" />

3.  Click the **Create New Connection & Credential** related link.

    <img src="./media/image10.png" style="width:5.84107in;height:3.30908in" />

4.  On the form, fill the required values.

5.  Click **Create**.

## Store details of the Jamf server:

## 

Create a server instance record to discover applications and store
details of the Jamf server.

**Role required:** admin

**Procedure**

1.  Navigate to **JAMF** \> **Server instances**.

2.  Click **New**.

    <img src="./media/image11.png" style="width:6.26389in;height:1.54722in" />

3.  On the form, fill the required values.

    <img src="./media/image12.png" style="width:6.26389in;height:1.08207in" />

4.  Right-click the form header and click **Submit**.

5.  Click the **Discover Now** related link to discover all applications
    and retrieve data from the server.

    <img src="./media/image13.png" style="width:6.26389in;height:1.45486in" />

6.  Discover and Store Data subflow is triggered. Data is retrieved and
    in stored in the Jamf tables.

**Note:** Currently, you can use only the default connection and
credential alias records that have been shipped along with the Jamf
Spoke.

Once subflow execution is complete, data should be populated in Jamf
spoke tables.

<img src="./media/image14.png"
style="width:6.26389in;height:2.71111in" />

Discovery subflow populates “*Applications*”, “*Groups*” and
“*Policies*” from Jamf server. Verify it from Jamf Modules.

<img src="./media/image15.png"
style="width:6.46565in;height:1.76336in" />

### Licensing:

Deciding license allocation type:

1.  If Group specified in Software Configuration (that is linked with
    catalog item) is of type **Mobile or**

**Computer**, the license check is performed on software entitlements
with **Allocation Type as Device**.

2.  If Group specified in Software Configuration (that is linked with
    catalog item) is of type **User**, the

license check is performed on software entitlements with **Allocation
Type as User.**

### Create licenses for distributed software using CSD 2.0:

Licenses and software counters are associated with the software model
and must be created if you want to track the license for software deployed by CSD
2.0.

**Role required:**

sn_csd.CSD Admin

You can create software licenses in CSD 2.0 for software items deployed

from the service catalog by CSD 2.0. If Allocations Available is greater
than 0, CSD 2.0 assumes that the license is available for the software.

**Procedure:**

1.  Navigate to **Client Software Distribution 2.0 \> Software
    Entitlements.**

2.  Click New.

   <img src="./media/image16.png" style="width:6.26389in;height:1.77361in" />

3.  On the form, fill the required values.

   <img src="./media/image17.png" style="width:6.26389in;height:2.64583in" />

4.  Click Submit.

The license is created with the required rights.

**Note:** If SAM is used for licensing, make sure to create required
licenses in Software Entitlement(alm_license) table.
  
  
  
## Lab 3:

### Configuring catalog item for an application:**

**Set up a software model:**

Using the applications discovered on the Jamf server, set up a software
model to manage licenses.

**Role required:** Ensure that the user has one of the two mentioned set
of roles.

-   sn_jamf_spoke.jamf_admin and sn_csd.CSD Admin

-   admin

You can link a Jamf application to an existing software model or create
a new model.

**Procedure:**

1.  If you are setting up a software model for the Jamf application,
    navigate to **Jamf \> Applications.**

    A list of applications or policies discovered on the Jamf server
    appears.

2.  Open the required record.

    <img src="./media/image18.png" style="width:5.15059in;height:2.10192in" />

3.  To link to an existing model, click the magnifying glass icon in the
    **CSD software**

   **Model(**while using CSD 2.0 license tables**)** field or **SAM
    software model(**while using SAM license tables**)** and select a
    model from the list.

    Form view for SAM licensing:

    <img src="./media/image19.png" style="width:4.09795in;height:1.69688in" />

    Form view for CSD 2.0 licensing:

    <img src="./media/image20.png" style="width:5.73462in;height:2.27986in" />

4.  To create a model, click **Create Software Model** under related
    link.

5.  Complete the software model fields.

6.  Click **Submit.**

The view returns to the Jamf Applications list.

**Define the Jamf configuration:**

Associate that software with a group through a Jamf configuration to create catalog items for Jamf software deployment or to configure your instance to
revoke softwarethrough Jamf.

**Role required:** sn_jamf_spoke.jamf_admin or admin

The Jamf configuration process associates software with Jamf groups.

To deploy software from a Jamf server, the user or device must be a member of a Jamf group
associated with an install deployment. CSD 2.0 enables you to revoke unentitled
software using the Jamf server when that software can be removed using
the installation group by removing user or device from it.


Users requesting revokable software from the Service Catalog also have
the ability to define lease start and stop dates and request lease
extensions.

Perform these steps for the Jamf applications:

1.  After following ‘**Set up a Software Model’** instructions for a
    given application, click “New” in “**Configurations**” on the
    applications’s form view or click “Create Software Configuration”
    from related links.


    <img src="./media/image21.png" style="width:4.93348in;height:1.96519in" />

2.  On the form, fill the required values.

    <img src="./media/image22.png" style="width:5.88085in;height:1.03991in" />

3.  Click Submit.

**Create a catalog item for the Jamf application:**

  Create a catalog item for an application you want to offer for
  distribution from the service catalog using the applications discovered on the Jamf server.

 **Role required:** Ensure that the user has one of the two mentioned
set of roles.

-   sn_jamf_spoke.jamf_admin, catalog_admin, and sn_csd.CSD Admin

-   admin

 **Procedure:**

1.  If you are creating a catalog item for the Jamf application,
    navigate to **Jamf \> Applications.**

2.  Open the required record.

3.  Click the **Create Catalog Item** related link.

 <img src="./media/image23.png" style="width:3.83553in;height:1.8846in" />

4.  On the form, fill the required values.

 <img src="./media/image24.png" style="width:6.16964in;height:4.3851in" />

5.  Right-click the form header and click **Save**.

6.  In the **CSD Catalog Item Fulfillment Configuration** tab, open the
    default record and provide these values as per your requirement.

  <img src="./media/image25.png" style="width:6.26389in;height:3.36111in" />

7.  Click Update.

Catalog item is created and is available for ordering from Service Catalog.

8.  To see all CSD 2.0 catalog items, navigate to **Client Software
    Distribution 2.0** \> **Software Items**.

**Note:** If you are unable to see any of the above fields or tabs,
configure the table's form view or related lists accordingly.

**Deployment process**

Order an application from a CSD 2.0 catalog item in the service catalog
triggers the

**Order Client Software** flow.

This process deploys an application to a user or device through a
service catalog order:

1.  If the **Skip approval** check box is cleared in the software
    catalog item, the Order Client Software flow sends the catalog
    request to the requesting user's manager for approval.

2.  If the **Check license compliance** check box is selected in the
    software catalog item, the flow performs a software license check
    from SAM or CSD 2.0 depending on your configuration. If there is no
    license available, the flow creates a catalog task to procure more
    licenses and assigns the task to the CSD Administrators group.

3.  The Order Client Software flow triggers the **Deploy Client
    Software** flow that in turn triggers the provider-specific
    **Deployment Flow** that is specified in the provider record.

*Catalog item order view:*

<img src="./media/image26.png" style="width:5.80921in;height:3.53541in" />

**Note:** The device must have its serial number populated in the device
table.

**Lease start and end dates:**

All software deployed by CSD 2.0 requires users to specify the beginning
date for the lease.

If the catalog item is configured for revocation (uninstall), the form
displays the Lease end field, which allows the requester to define an end date and time for
the lease. The system validates user input in these fields to ensure that the dates
selected define a future window. The Lease end field is not mandatory and can be left
blank to order software with no end date.

**Viewing Requested Software:**

Navigate to Client **Software Distribution 2.0 \> Requested Software.**

<img src="./media/image27.png" style="width:6.96735in;height:1.70322in" />

## Bonus Lab:

**Extending Software Lease:**

Users of software deployed by CSD 2.0 can request the extension of a
lease window, if

the software is revocable by a software distribution system.

Ensure that the user has one of the two mentioned set of roles.

-   itil and sn_csd.CSD Admin

-   itil and sn_csd.CSD User

**Procedure:**

1.  Navigate to **Client Software Distribution 2.0 \> Requested
    Software**. The list shows only the softwares you have requested
    from the service catalog.

2.  Select the record for the installed software whose lease you want to
    extend.

3.  Under Related Links, click **Extend Lease**

4.  In the dialog box that appears, select a new lease end date in the
    calendar and

click **OK**. You must select a date later than the current date.

  <img src="./media/image28.png" style="width:5.55848in;height:2.80759in" />

**Revoke Software:**

Request for revoking requested software will be automatically sent to
the Jamf server on the specified Lease End Date. You can follow below
steps to revoke it manually before that:

Ensure that the user has one of the two mentioned set of roles.

-   itil and sn_csd.CSD Admin

-   itil and sn_csd.CSD User

**Procedure:**

1.  Navigate to Client Software Distribution 2.0 \> Requested Software.
    The list shows only the softwares you have requested from the
    service catalog.

2.  Select the record for the installed software whose lease you want to
    extend.

3.  Under Related Links click **Revoke Software**

<img src="./media/image29.png" style="width:5.49269in;height:1.50166in" />

