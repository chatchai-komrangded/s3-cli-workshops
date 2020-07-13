# Amazon CLI S3 workshops example
## HOW TO: Using Amazon CLI for Amazon S3
## Reference
https://docs.aws.amazon.com/cli/latest/userguide/cli-services-s3-commands.html
### Pre-requisites:
- Provision Cloud9 IDE
---
## Step to follow How to Amazon CLI on S3
 
 ## 1.) Manage buckets

High-level aws s3 commands support common bucket operations, such as creating, listing, and deleting buckets.

## Create a bucket
Use the s3 mb command to create a bucket. Bucket names must be globally unique and should be DNS compliant. Bucket names can contain lowercase letters, numbers, hyphens, and periods. Bucket names can start and end only with a letter or number, and cannot contain a period next to a hyphen or another period.

    aws s3 mb s3://chatkom-global-uniques

## List your buckets
Use the s3 ls command to list your buckets. Here are some examples of common usage.

    aws s3 ls
    aws s3 ls s3://chatkom-global-uniques

You can filter the output to a specific prefix by including it in the command. The following command lists the objects in chatkom-global-uniques/path (that is, objects in chatkom-global-uniques filtered by the prefix path/).

    aws s3 ls s3://chatkom-global-uniques/path/

 ## Delete a bucket
To remove a bucket, use the s3 rb command.
    
    aws s3 rb s3://chatkom-global-uniques

By default, the bucket must be empty for the operation to succeed. To remove a non-empty bucket, you need to include the --force option.

The following example deletes all objects and subfolders in the bucket and then removes the bucket.

    aws s3 rb s3://chatkom-global-uniques --force

 ## 2.) Manage objects
The high-level aws s3 commands make it convenient to manage Amazon S3 objects. The object commands include s3 cp, s3 ls, s3 mv, s3 rm, and s3 sync.

The cp, ls, mv, and rm commands work similarly to their Unix counterparts and enable you to work seamlessly across your local directories and Amazon S3 buckets. The sync command synchronizes the contents of a bucket and a directory, or two buckets.

You can also specify a nondefault storage class (REDUCED_REDUNDANCY or STANDARD_IA) for objects that you upload to Amazon S3. To do this, use the --storage-class option.

    aws s3 cp file.txt s3://chatkom-global-uniques/ --storage-class REDUCED_REDUNDANCY

The s3 sync command uses the following syntax. Possible source-target combinations are:

- Local file system to Amazon S3

- Amazon S3 to local file system

- Amazon S3 to Amazon S3    

The following example synchronizes the contents of an Amazon S3 folder named path in my-bucket with the current working directory. s3 sync updates any files that have a different size or modified time than files with the same name at the destination. The output displays specific operations performed during the sync. Notice that the operation recursively synchronizes the subdirectory MySubdirectory and its contents with s3://chatkom-global-uniques/path/MySubdirectory.

    aws s3 sync . s3://chatkom-global-uniques/path

The following example, which extends the previous one, shows how this works
    
    // Delete local file
    rm ./MyFile1.txt

    // Attempt sync without --delete option - nothing happens
    aws s3 sync . s3://chatkom-global-uniques/path

    //  Sync with deletion - object is deleted from bucket
    aws s3 sync . s3://chatkom-global-uniques/path --delete

    // Delete object from bucket
    aws s3 rm s3://chatkom-global-uniques/path/MySubdirectory/MyFile3.txt

    // Sync with deletion - local file is deleted
    aws s3 sync s3://chatkom-global-uniques/path . --delete

    // Sync with Infrequent Access storage class
    aws s3 sync . s3://chatkom-global-uniques/path --storage-class STANDARD_IA

You can use the --exclude and --include options to specify rules that filter the files or objects to copy during the sync operation. By default, all items in a specified folder are included in the sync. Therefore, --include is needed only when you have to specify exceptions to the --exclude option (that is, --include effectively means "don't exclude"). The options apply in the order that's specified, as shown in the following example.

    Local directory contains 3 files:
    MyFile1.txt
    MyFile2.rtf
    MyFile88.txt

    aws s3 sync . s3://chatkom-global-uniquespath --exclude "*.txt"
    aws s3 sync . s3://chatkom-global-uniques/path --exclude "*.txt" --include 
    aws s3 sync . s3://chatkom-global-uniques/path --exclude "*.txt" --include "MyFile*.txt" --exclude "MyFile?.txt"

The --exclude and --include options also filter files or objects to be deleted during an s3 sync operation that includes the --delete option. In this case, the parameter string must specify files to exclude from, or include for, deletion in the context of the target directory or bucket. The following shows an example.

    Assume local directory and s3://my-bucket/path currently in sync and each contains 3 files:
    MyFile1.txt
    MyFile2.rtf
    MyFile88.txt

    // Delete local .txt files
    rm *.txt

    // Sync with delete, excluding files that match a pattern. MyFile88.txt is deleted, while remote MyFile1.txt is not.
    aws s3 sync . s3://my-bucket/path --delete --exclude "my-bucket/path/MyFile?.txt"

    // Delete MyFile2.rtf
    aws s3 rm s3://my-bucket/path/MyFile2.rtf

    // Sync with delete, excluding MyFile2.rtf - local file is NOT deleted
    aws s3 sync s3://my-bucket/path . --delete --exclude "./MyFile2.rtf"

    // Sync with delete, local copy of MyFile2.rtf is deleted
    aws s3 sync s3://my-bucket/path . --delete

As previously mentioned, the s3 command set includes cp, mv, ls, and rm, and they work in similar ways to their Unix counterparts. The following are some examples.

    // Copy MyFile.txt in current directory to s3://my-bucket/path
    aws s3 cp MyFile.txt s3://my-bucket/path/

    / /Move all .jpg files in s3://my-bucket/path to ./MyDirectory
    aws s3 mv s3://my-bucket/path ./MyDirectory --exclude "*" --include "*.jpg" --recursive

    // List the contents of my-bucket
    aws s3 ls s3://my-bucket

    // List the contents of path in my-bucket
    aws s3 ls s3://my-bucket/path/

    // Delete s3://my-bucket/path/MyFile.txt
    aws s3 rm s3://my-bucket/path/MyFile.txt

    // Delete s3://my-bucket/path and all of its contents
    aws s3 rm s3://my-bucket/path --recursive


When you use the --recursive option on a directory or folder with cp, mv, or rm, the command walks the directory tree, including all subdirectories. These commands also accept the --exclude, --include    





