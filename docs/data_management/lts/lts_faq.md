# LTS FAQ

## What Are Valid Bucket Names In LTS?

Bucket names must be comprised only of lowercase letters, numbers, and hyphens. No capital letters or underscores are allowed. Trying to create a bucket with the disallowed characters will return an error.

Bucket names in LTS _must_...

- be globally unique across all of LTS
    - We recommend adding a universally unique identifier (UUID) after the desired name to ensure uniqueness.
    - Example: change the generic `my-bucket` to the unique `my-bucket-1499e6d5-b719-4cc1-831d-fe1b25970b3b`.
    - The site <https://uuidgenerator.net> can be used to generate UUIDs.
    - Use "Version 4" UUIDs as they are fully randomized.
- be between 3 and 63 characters long;
- have only lowercase letters, numbers, dots `.`, and hyphens `-`
- begin and end with a letter or number
- have no consecutive dots (`..` is not allowed)
- not be formatted like an IP address (e.g., not like `127.0.0.1`)

The following rules are not required on LTS, but recommended for compatibility with Amazon AWS S3. Bucket names in LTS _should_...

- not start with specific prefixes (these are reserved):
    - `xn--`
    - `sthree-`
    - `amzn-s3-demo-`
- not end with specific suffixes (these are reserved):
    - `-s3alias`
    - `--ol-s3`
    - `.mrap`
    - `--x-s3`
- not contain dots `.` if used with Amazon AWS S3 Transfer Acceleration

<!-- markdownlint-disable MD046 -->
!!! example

    - `my-bucket`: not good, probably won't work
    - `my-bucket-1234`: acceptable, but may not work
    - `specific-research-core-name`: better, likely to work, Core names are probably unique
    - `specific-research-core-name-1499e6d5-b719-4cc1-831d-fe1b25970b3b`: excellent, guaranteed to work
<!-- markdownlint-enable MD046 -->

## What Information Do I Need to Keep Track of For Managing LTS allocations?

Please store the following information carefully for your LTS allocation(s).

- Access Key (looks like `AKIAIOSFODNN7EXAMPLE`) grants you access to your allocation. This is equivalent to a username in other systems.
- Secret Key (looks like `wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY`) grants you access to your allocation. This is equivalent to a password in other systems and must be kept secret. Do not share this information with anyone.
- IAM Name allows others to grant you access to their allocations via [Bucket Policy](./iam_and_policies.md#sharing-buckets).
    - For individual allocations the IAM Name will be your UAB Campus email address (e.g., `blazerid@uab.edu`).
    - For shared allocations the IAM Name may vary, but should be related to your lab or Core name. When creating your allocation, you may be asked to provide this name as part of the creation process.

<!-- markdownlint-disable MD046 -->
!!! note

    Early adopters of LTS will have an IAM Name like `blazerid` instead of `blazerid@uab.edu` due to an error on our part.
<!-- markdownlint-enable MD046 -->

These should come to you in an email when your allocation is created. The Access and Secret Keys will be contained in a text file in UAB Box. A link will be provided in the email. The text file will be deleted within 7 days.

If you lose track of any of this information, please [Contact Support](../../help/support.md).

## What are Access and Secret Keys?

Access keys are like a username, and secret keys are like a password, please treat them accordingly. You will get separate sets of keys for each allocation you manage or are responsible for.

## Can I Share My Allocation Access Keys With Other People?

You should never share access keys with anyone. These should be treated similarly to your BlazerID and password. Sharing keys creates a point of vulnerability and if they fall into a nefarious actor's hands, all buckets that allocation owns and the data in them can be deleted.

In some cases, you may not be actively managing data in a bucket even though you own the allocation which owns a shared bucket. Instead of sharing keys with a data steward, instead that steward should be given admin-esque permissions on the required bucket via a policy file.

## How Should I Organize My LTS Shared Allocation?

This is ultimately up to the bucket owner, but there are a couple of single-bucket solutions depending on your specific use-case for LTS:

1. Semi-synced copy of everything in the project space.
    - **General permissions:** Data stewards and the bucket owner would have permission to delete any files. All other users would be able to upload and download files only. All users would be able to see all files uploaded by all other users to that bucket./
    - **Purpose:** This fulfills more of a pure backup role compared to option 2. While all users can upload files
    - **Benefits:** The policy file for these permissions is much simpler to create and manage. Limits the number of people who can remove files that might be needed down the line.
    - **Drawbacks:** Needing to ask a steward or the bucket owner to delete individual files introduces friction to data management.
    - [Example Policy File](res/example-synced-project-policy.json){: download="example-synced-project-policy.json" }
1. Active and collaborative external storage. All users would have a specific prefix/folder they have complete control where they can add or remove data at will.
    - **General permissions:** Data stewards and the bucket owner would have permission to delete any files. Regular users would only be able to upload to and delete files from their owned prefix/folder. All users would be able to see and download any files from any other user.
    - **Purpose:** This satisfies the need for expanded storage accessible from Cheaha (via the terminal or Globus). All users have their own space they can use as they see fit within the bucket for extra storage while still being able to access, but not alter, files from other users in cases they need to be shared. Part of the bucket, or a separate bucket entirely, can also be used as a backup for old or current datasets where users only have read permissions.
    - **Benefits:** How the bucket can be used is much more malleable and up to the individual users. Empowers them to add and remove data from their own prefix/folder without oversight from stewards or the bucket owner.
    - **Drawbacks:** The policy file is more difficult to craft and manage when researchers needed to be added or removed from the bucket. Allowing users to delete their uploaded data at their discretion may conflict with the owner's view of those data.
    - [Example Policy File](res/example-active-external-storage-policy.json){: download="example-active-external-storage-policy.json" }

While these are two simple solutions, a combination of both can be implemented with some clever crafting of the policy file. As well, you could take advantage of both solutions with multiple buckets. Keep in mind that data in all buckets contribute towards the total storage allocation equally. Once an allocation's storage quota is reached, no files can be added to any bucket owned by that allocation until files are removed.

<!-- TODO cross-link to new content-->

## Are Automatic Backups to LTS Available?

Automatic backups are not available by default. If you would like to periodically sync your bucket to a directory on your local machine or Cheaha, you will need to set up a cron task to submit a Slurm job that will run a sync. IF you would like to implement this for your own bucket, please [contact us](../../index.md#how-to-contact-us).

## Why Can I Not Interact With A File In My Bucket?

While S3's object storage system does not have POSIX permissions seen in a Linux system entirely, we have found that users who upload files to a shared space have ownership permissions on those objects, and the bucket owner and stewards cannot interact with those objects by default. Instead, owners and stewards need to be given explicit permissions to move or delete all objects in a bucket. This can be dealt with by adding the following sections to the policy file:

``` json
{
    "Sid": "admin",
    "Effect": "Allow",
    "Principal": {
        "AWS": [
            "arn:aws:iam:::user/example_core",
            "arn:aws:iam:::user/allocation_owner@uab.edu"
        ]
    },
    "Action": [
        "s3:*"
    ],
    "Resource": [
        "arn:aws:s3:::bucket",
        "arn:aws:s3:::bucket/*"
    ]
},
{
    "Sid": "data-steward",
    "Effect": "Allow",
    "Principal": {
        "AWS": [
            "arn:aws:iam:::user/steward_1@uab.edu"
        ]
    },
    "Action": [
        "s3:ListBucket",
        "s3:GetObject",
        "s3:PutObject",
        "s3:DeleteObject",
        "s3:GetBucketPolicy",
        "s3:PutBucketPolicy"
    ],
    "Resource": [
        "arn:aws:s3:::bucket",
        "arn:aws:s3:::bucket/*"
    ]
}
```

## How Can I Share A Bucket With All LTS Users?

The following policy file will give read permission to all LTS users for all objects in a bucket:

``` json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "admin",
            "Effect": "Allow",
            "Principal": {
                "AWS": [
                    "arn:aws:iam:::user/allocation_owner@uab.edu"
                ]
            },
            "Action": [
                "s3:*"
            ],
            "Resource": [
            "arn:aws:s3:::bucket",
            "arn:aws:s3:::bucket/*"
            ]
        },
        {
            "Sid": "read-only-all",
            "Effect": "Allow",
            "Principal": {
                "AWS": [
                    "arn:aws:iam:::user/*"
                ]
            },
            "Action": [
                "s3:ListBucket",
                "s3:GetObject"
            ],
            "Resource": [
            "arn:aws:s3:::bucket",
            "arn:aws:s3:::bucket/*"
            ]
        }
    ]
}
```

## Can I Change Permissions On A Bucket Via Globus?

As of now, there is no way to change permissions on a bucket via [Globus](../transfer/globus.md). The only way to change permissions is via the command line.

<!-- TODO cross-link to new content-->

## Is LTS compatible with Amazon S3?

Yes! LTS is compatible with Amazon S3 (Simple Storage Service). Most software solutions that work with Amazon AWS S3 will also work with LTS. Generally, you will need to instruct the software where the interface is located by supplying an Endpoint URL. Our Endpoint URL is `https://s3.lts.rc.uab.edu/`.

We recommend the following software:

- [Globus](../transfer/globus.md) for general-use data transfers. See our [Tutorials](../transfer/tutorial/index.md).
- [s5cmd](../lts/interfaces.md#s5cmd) for high-throughput data transfers.
- [s3cmd](../lts/interfaces.md#s3cmd) for managing bucket policies. Note that for some use cases, you may not need a bucket policy, and Globus will be enough to share data.

If you are unsure of how to make the most of LTS, please [Contact Support](../../help/support.md)

## How Do Folders Work in LTS and S3?

LTS is an S3-compatible storage. S3 does not have a concept of folders, only buckets and objects, with a flat structure. All buckets are siblings, and objects go in buckets. All objects in a given bucket are siblings. Buckets never go in buckets. The flat structure gives S3 stable performance and low cost at very large scales of data.

Almost all software solutions that interact with S3 have a way to simulate hierarchical folder structures. Zero-byte objects with a trailing slash `/` character are treated as though they were folders, e.g., `folder/`. Any objects prefixed with `folder/` will be treated as though contained within a folder called `folder`. In the simulation, buckets are also treated as folders.

### Example

The following table gives an example of how software, like [Globus](../transfer/globus.md), simulates folders when looking at S3-compatible storage. The first row shows the bucket called `bucket`, which contains all of the objects in the example. The remaining rows in the table each describes one of the example objects.

{{ read_csv('data_management/lts/res/s3-folder-example.csv', keep_default_na=False) }}

- **Filesystem Equivalent**: The equivalent tree structure if these objects were in a hierarchical file system, like on your personal computer or Cheaha. This also shows the relative locations of objects when using, e.g., Globus.
- **Internal S3 Name**: The name of the object as known to S3. Note the `folder/` prefix on the last object.
- **Display Name**: The name displayed in software like Globus.
- **Behaves Like**: What this object or bucket behaves like in the simulated tree structure.
- **Is Actually**: Whether the entity in this row represents a bucket or object.
