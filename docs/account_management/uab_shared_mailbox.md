# UAB Shared Mailbox

Shared Mailboxes are a feature supplied by UAB Enterprise IT to give multiple people access to the same shared collection of emails and to provide a single point of contact for organizations at UAB. In Research Computing, Shared Mailboxes may be used as service accounts for third-party services, including to create Globus Collections that are independent of individual researchers.

## Prepare to Work with Shared Mailboxes

<!-- markdownlint-disable MD046 -->
!!! note

    The form is not a fully automated process. When you click "Submit" a ticket will be created and then followed up on manually. This process can take time, so please plan accordingly.
<!-- markdownlint-enable MD046 -->

1. Determine your organization's Active Directory (AD) organizational unit (OU). Generally, this is an uppercase string of letters with at most four characters, like `ITRC`. If you do not have an OU, you will need to have one created by UAB Enterprise IT.
1. Whoever performs the following steps must be an administrator of your OU.
1. Navigate to the [Active Directory OU Management form](https://uabprod.service-now.com/service_portal?id=sc_cat_item&sys_id=b66cdb171b98b304997443f8cd4bcbf2) in ServiceNow.
1. In the "Object Type" drop-down field select "Resource Object - Shared Mailbox".
1. You should see a new field appear titled "Request Type". From here you can now work with Shared Mailboxes.

- [List Existing Shared Mailboxes](#list-existing-shared-mailboxes)
- [Create a New Shared Mailbox](#create-a-new-shared-mailbox)
- [Delete an Existing Shared Mailbox](#delete-a-shared-mailbox)

## List Existing Shared Mailboxes

1. [Prepare to Work with Shared Mailboxes](#prepare-to-work-with-shared-mailboxes).
1. In the "Request Type" drop-down field select "List/Delete resource object". New fields will appear.
1. In the "OU Name" drop-down field select your OU. If you have no existing Shared Mailboxes, then you should see a "No records found" message. Otherwise, you will see a list of Shared Mailboxes.

## Create a new Shared Mailbox

1. [Prepare to Work with Shared Mailboxes](#prepare-to-work-with-shared-mailboxes).
1. Under "Request Type" drop-down field select "Create a new resource object with mailbox in uabOther OU".
1. In the "OU Name" drop-down field select your OU.
1. Enter the desired Shared Mailbox name in the "Object Name" field. When you have finished entering the desired name, click outside the field. The system will check if the desired name is available and provide feedback. Additionally, the "Full Object Name" field will update to show you what the full name will be in AD.
1. Enter a desired display name in the "Display Name" field. This name can be anything you want. We recommend entering a name that is relevant and memorable.
1. Enter BlazerIDs for people in the "Full Access", "Send As", and "Send on Behalf of" delegate fields. Permissions available for people in each field are summarized below.

    {{ read_csv('account_management/res/shared_mailbox_delegate_permissions.csv', keep_default_na=False) }}

    More complete details on permissions are available at the following [Microsoft Learn page for Exchange Server](https://learn.microsoft.com/en-us/exchange/recipients/mailbox-permissions?view=exchserver-2019).

    <!-- markdownlint-disable MD046 -->
    !!! important

        For a delegate to be able to both read and send emails, you must put their BlazerID in both the "Full Access" and "Send As" (or "Send on Behalf of") fields.
    <!-- markdownlint-enable MD046 -->

1. Review the available option checkboxes and select any relevant to your needs.

    - **"Check if when..."**: Check this if you want email copies sent to the sender's personal mailbox. This applies whenever "Send As" is used.
    - **"Check to add shared mailbox alias"**: This will let you create an alias for the Shared Mailbox. If you are reading this page for the purposes of managing Globus collections, this is not necessary.

1. When you have finished filling the form, click the "Submit" button. A service request will be created, it may take some time for the Shared Mailbox to be created.

## Delete a Shared Mailbox

1. [List Existing Shared Mailboxes](#list-existing-shared-mailboxes).
1.
