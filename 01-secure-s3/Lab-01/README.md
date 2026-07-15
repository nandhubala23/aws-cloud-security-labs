# AWS S3 Mini Lab – Create, Copy, Move, Create Folder & Delete Bucket Using AWS CLI

## Objective

The objective of this mini lab is to learn how to perform basic Amazon S3 operations using the AWS Command Line Interface (AWS CLI). By the end of this lab, you will be able to:

- Configure AWS CLI
- Verify AWS credentials
- List S3 buckets and objects
- Create a logical folder in an S3 bucket
- Copy objects within a bucket
- Move objects between locations
- Delete objects and folders
- Delete an S3 bucket using AWS CLI

---

## Architecture

```
AWS CLI
    │
    ▼
Amazon S3
└── aurora-aws-bucket-lab01
    ├── obj1.ARW
    ├── obj2.RAF
    └── my-bucket/
```

---

## AWS Services Used

- **Amazon S3** – Object storage service used to store and manage files.
- **AWS Identity and Access Management (IAM)** – Provides secure authentication and authorization for the AWS CLI.
- **AWS CLI** – Command-line tool used to interact with AWS services.

---

## Prerequisites

- AWS Account
- IAM User with S3 permissions
- AWS CLI installed
- AWS CLI configured

Configure the AWS CLI:

```bash
aws configure
```

Example:

```text
AWS Access Key ID: ***************
AWS Secret Access Key: ****************
Default region name: ap-south-1
Default output format: json
```

Verify the configured IAM user:

```bash
aws sts get-caller-identity
```

Example Output:

```json
{
    "UserId": "AIDAYZYM643LPELTKQUR2",
    "Account": "605080708822",
    "Arn": "arn:aws:iam::605080708822:user/Nandhu"
}
```

---

## Implementation Steps

### Step 1 – List Available Buckets

```bash
aws s3 ls
```

Example Output

```text
2026-07-15 15:10:14 aurora-aws-bucket-lab01
```

---

### Step 2 – List Objects in the Bucket

```bash
aws s3 ls s3://aurora-aws-bucket-lab01
```

Example Output

```text
PRE aws/
11141120 obj1.ARW
50221056 obj2.RAF
```

---

### Step 3 – Create a Folder

Amazon S3 does not have real folders. A folder is represented by an object key ending with `/`.

```bash
aws s3api put-object \
    --bucket aurora-aws-bucket-lab01 \
    --key my-bucket/
```

Verify:

```bash
aws s3 ls s3://aurora-aws-bucket-lab01
```

Output

```text
PRE my-bucket/
obj1.ARW
obj2.RAF
```

---

### Step 4 – Copy an Object

Copy **obj1.ARW** into **my-bucket**.

```bash
aws s3 cp \
s3://aurora-aws-bucket-lab01/obj1.ARW \
s3://aurora-aws-bucket-lab01/my-bucket/obj1.ARW
```

Verify

```bash
aws s3 ls s3://aurora-aws-bucket-lab01/my-bucket/
```

---

### Step 5 – Move an Object

Move **obj2.RAF** into **my-bucket**.

```bash
aws s3 mv \
s3://aurora-aws-bucket-lab01/obj2.RAF \
s3://aurora-aws-bucket-lab01/my-bucket/obj2.RAF
```

Verify

```bash
aws s3 ls s3://aurora-aws-bucket-lab01/my-bucket/
```

Expected Output

```text
obj1.ARW
obj2.RAF
```

---

### Step 6 – Delete Objects

Delete the copied objects.

```bash
aws s3 rm s3://aurora-aws-bucket-lab01/my-bucket/obj1.ARW
```

```bash
aws s3 rm s3://aurora-aws-bucket-lab01/my-bucket/obj2.RAF
```

---

### Step 7 – Delete the Folder

Delete the folder placeholder object.

```bash
aws s3 rm s3://aurora-aws-bucket-lab01/my-bucket/
```

---

### Step 8 – Delete the Bucket

A bucket must be empty before it can be deleted.

```bash
aws s3api delete-bucket \
--bucket aurora-aws-bucket-lab01
```

Verify

```bash
aws s3 ls
```

If no buckets are displayed, the deletion was successful.

---

## Security Configuration

- Configured AWS CLI using IAM Access Keys.
- Verified IAM identity using:

```bash
aws sts get-caller-identity
```

- Used an IAM user with Amazon S3 permissions.
- Performed all operations through authenticated AWS CLI commands.
- No public access or bucket policy modifications were required for this lab.

---

## Validation

The lab is considered successful if the following validations pass:

- ✅ AWS CLI authentication verified.
- ✅ Bucket listed successfully.
- ✅ Objects displayed inside the bucket.
- ✅ Folder created successfully.
- ✅ Object copied into the folder.
- ✅ Object moved into the folder.
- ✅ Objects deleted successfully.
- ✅ Folder removed.
- ✅ Bucket deleted successfully.

---

## Lessons Learned

- Learned how to configure AWS CLI with IAM credentials.
- Understood how Amazon S3 stores objects using prefixes instead of real folders.
- Practiced listing buckets and objects using AWS CLI.
- Learned the difference between **copy (`aws s3 cp`)** and **move (`aws s3 mv`)** operations.
- Understood that an S3 bucket must be empty before it can be deleted.
- Gained hands-on experience managing Amazon S3 entirely from the command line.

---

## Cleanup

Delete any remaining resources to avoid unnecessary AWS charges.

Delete objects:

```bash
aws s3 rm s3://aurora-aws-bucket-lab01 --recursive
```

Delete the bucket:

```bash
aws s3api delete-bucket \
--bucket aurora-aws-bucket-lab01
```

Verify cleanup:

```bash
aws s3 ls
```

If no buckets are listed, the cleanup is complete.
