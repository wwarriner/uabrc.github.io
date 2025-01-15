# Using Globus to Manage LTS Allocations

## Prerequisites

Before using any of these tutorials, you will need to set up an [Individual LTS Allocation](../storage.md#how-do-i-request-an-individual-long-term-storage-allocation) and. If you represent a group such as a Lab or Research Core, you will also need to set up a [Shared LTS Allocation](../storage.md#how-do-i-request-shared-storage).

## How Do I Prepare to Use LTS in Globus?

1. [Get Onto the Globus Web App](../transfer/tutorial/globus_individual_tutorial.md#how-do-i-get-onto-the-globus-web-app).
1. [Configure your LTS Access Keys](#how-do-i-add-or-change-lts-access-keys), unless you have already done so. Be mindful that you may have multiple sets of keys, one for each role and storage allocation you have.
    - Individual allocation
    - Shared lab allocation
    - Shared core allocation
1. [Find the LTS Collection in the File Manager](../transfer/tutorial/globus_individual_tutorial.md#how-do-i-find-uab-storage-mapped-collections).

## How Do I Add or Change LTS Access Keys?

1. [Get Onto the Globus Web App](../transfer/tutorial/globus_individual_tutorial.md#how-do-i-get-onto-the-globus-web-app).
1. [Find the UAB LTS Collection](../transfer/tutorial/globus_individual_tutorial.md#how-do-i-find-uab-storage-mapped-collections).
1. Click the "Credentials" tab in the Collection details page, highlighted in the screenshot below.

    ![UAB LTS Collection details page](./images/gl-access-keys/001-details-page.png)

1. Enter your [UAB LTS Access Key and Secret Key](../lts/lts_faq.md#what-are-access-and-secret-keys) here. If you are working to configure [Guest Collections](../transfer/tutorial/globus_organization_tutorial.md#how-do-i-create-a-collection) for a Shared LTS allocation, such as for a Lab or Research Core, then you will need to enter the shared allocation keys of your individual allocation keys.

    ![UAB LTS Collection details page credentials tab showing entry form](./images/gl-access-keys/002-credentials-tab.png)

1. If the keys have been entered successfully you will be redirected to the UAB LTS Collection details page on the "Credentials" tab. There will be a new entry in the "Access Credentials" space. You should see the same Access Key you entered.

    - To delete the keys, click the "trash can" icon.
    - To update the keys, click the "gear" icon.

    <!-- markdownlint-disable MD046 -->
    !!! important

        At this time, you can only have keys for one allocation active at a time in the UAB LTS Collection. To manage a different LTS allocation using Globus, you will need to delete the existing keys at this page and enter new keys.
    <!-- markdownlint-enable MD046 -->

    ![UAB LTS Collection details page credentials tab showing new entry](./images/gl-access-keys/003-credentials-tab-finished.png)

## How Do I Create a Bucket?

1. [Prepare to Use LTS in Globus](#how-do-i-prepare-to-use-lts-in-globus).
1. [Create a New Folder](../transfer/tutorial/globus_individual_tutorial.md#how-do-i-modify-files-and-folders).
    - Folders created at the root level of your LTS allocation become buckets, so you will need to be mindful of [LTS Bucket Name Rules](../lts/lts_faq.md#what-are-valid-bucket-names-in-lts) when choosing a name. Bucket folders behave like any other folders when transferred with Globus.
    - [Folders created inside buckets](#how-do-i-create-a-folder-in-a-bucket) behave like regular folders when using the Globus interface.

        <!-- markdownlint-disable MD046 -->
        !!! info

            Folders in LTS are implemented differently from traditional filesystems. If you are interested in learning more, see [How Do Folders Work in LTS and S3?](../lts/lts_faq.md#how-do-folders-work-in-lts-and-s3).
        <!-- markdownlint-enable MD046 -->

## How Do I Create a Folder in a Bucket?

1. [Prepare to Use LTS in Globus](#how-do-i-prepare-to-use-lts-in-globus).
1. Open the bucket where you want to create a folder.
1. Click the [Create New Folder button](../transfer/tutorial/globus_individual_tutorial.md#how-do-i-modify-files-and-folders) to create a new folder.

## How Do I Share a Bucket or Folder as a Collection?

## How Do I Manage Bucket Policies?

Globus has no concept or interface for managing bucket policies, instead sharing is done using [Collections](#how-do-i-share-a-bucket-or-folder-as-a-collection).

A Collection can be thought of as a gateway. The gateway is prepared in advance with your credentials. Then, you choose who you want to have access to your data through that gateway. Data is then shared on your behalf, using your credentials.

<!-- markdownlint-disable MD046 -->
!!! info

    Buckets in Globus have no policy applied and the most restrictive access controls possible. If you want to allow others to access the bucket outside of Globus, you will still need to use [policies](../lts/policies.md)
<!-- markdownlint-enable MD046 -->

For Cores and other organizations with customers, sharing LTS through Collections means your customers, and most of your staff, do not have to work with LTS directly.
