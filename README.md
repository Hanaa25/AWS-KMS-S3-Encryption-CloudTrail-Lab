# AWS Key Management Service (KMS) – Hands-On Lab

## 📌 Overview

This hands-on lab demonstrates how **AWS Key Management Service (AWS KMS)** can be used to create and manage encryption keys, protect objects stored in **Amazon S3**, control key usage permissions, and monitor encryption-related activity through **AWS CloudTrail**.

The lab provides practical experience with **server-side encryption using AWS KMS keys (SSE-KMS)** and demonstrates how encryption, access control, object permissions, and audit logging work together within AWS.

---

## 🎯 Objectives

By completing this lab, I gained practical experience in:

* Creating and configuring a customer managed AWS KMS key.
* Using an AWS KMS key to encrypt an Amazon S3 object.
* Understanding how S3 interacts with KMS during object decryption.
* Configuring Amazon S3 public access and Object Ownership settings.
* Understanding why SSE-KMS encrypted objects cannot simply be accessed through a public S3 URL.
* Monitoring KMS and S3 activity using AWS CloudTrail.
* Identifying the KMS Key ID and uploaded object name within CloudTrail logs.
* Managing KMS key users and their permissions.

---

## 🛠️ AWS Services Used

| Service            | Purpose                               |
| ------------------ | ------------------------------------- |
| **AWS KMS**        | Create and manage the encryption key  |
| **Amazon S3**      | Store and encrypt the uploaded object |
| **AWS CloudTrail** | Record and monitor API activity       |
| **IAM**            | Control access to the KMS key         |

---

# 🔐 Lab Implementation

## 1. Create a Customer Managed KMS Key

A symmetric customer managed KMS key was created with the following configuration:

* **Key type:** Symmetric
* **Alias:** `myFirstKey`
* **Purpose:** Encryption of S3 data
* **Key administrators:** Lab IAM user
* **Key users:** Lab IAM user

The Key ID was also recorded for later verification in CloudTrail logs.

### Screenshot

![Create KMS Key](screenshots/01-Create%20key.png)

---

## 2. Configure CloudTrail

A CloudTrail trail was configured to store AWS activity logs in an Amazon S3 bucket.

The configuration included monitoring:

* Management events
* Data events
* Insights events
* API call rate
* API error rate

### Screenshot

![Create CloudTrail Trail](screenshots/02-Create%20trail.png)

---

## 3. Upload and Encrypt an S3 Object

An image file was uploaded to the S3 bucket and encrypted using:

**Server-side encryption with AWS Key Management Service keys (SSE-KMS)**

The previously created `myFirstKey` was selected as the KMS encryption key.

### Screenshot

![Upload Encrypted Image](screenshots/03-Upload%20image%20file%20with%20encrypted%20key.png)

---

## 4. Access the Encrypted Object

The encrypted image was successfully opened through the AWS Management Console.

This demonstrates that authorized access allows Amazon S3 to interact with AWS KMS to decrypt the object.

### Screenshot

![Open Encrypted Image](screenshots/03-Open%20image%20from%20consol.png)

---

## 5. Test Direct S3 URL Access

The S3 object URL was accessed directly through a browser.

Initially, access was denied because public access was blocked.

### Screenshot

![Access Denied](screenshots/04-Access%20Denied.png)

---

## 6. Configure S3 Public Access and Object Ownership

To demonstrate S3 access behavior, the bucket configuration was modified by:

* Disabling Block Public Access.
* Enabling ACLs.
* Updating Object Ownership settings.

### Screenshots

![Deselect Block Public Access](screenshots/05-Deselect%20block%20public%20access.png)

![Object Ownership](screenshots/06-Successfully%20edited%20Object%20Ownership..png)

---

## 7. Make the Object Public

The uploaded image was made public using an S3 ACL.

### Screenshot

![Make Public Object](screenshots/07-Make%20public%20object.png)

However, because the object uses **SSE-KMS encryption**, accessing it through a public URL still requires authenticated requests using **AWS Signature Version 4**.

This demonstrates an important distinction between:

**S3 object permissions**
and
**KMS encryption permissions**

Making an object publicly accessible does not bypass the encryption requirements imposed by SSE-KMS.

---

## 8. Verify Server-Side Encryption

The object access behavior was examined to understand the requirement for authenticated requests when accessing SSE-KMS encrypted objects.

### Screenshot

![Server Side Encryption Request](screenshots/08-%20Request%20specific%20server%20side%20Encryption.png)

---

# 📊 9. Monitor KMS Activity Using CloudTrail

CloudTrail logs stored in the S3 bucket were examined to identify activity associated with the encryption operation.

The relevant log file was opened and inspected in JSON format.

### Screenshot

![CloudTrail Log File](screenshots/09-Log%20file.png)

---

## 🔎 10. Identify the KMS Key ID in the Log

The KMS Key ID created earlier was located within the CloudTrail log.

### Screenshot

![KMS Key ID in CloudTrail](screenshots/11-Open%20log%20file%20contain%20keyID.png)

---

## 🔎 11. Identify the Uploaded Object

The name of the uploaded image was also located within the same CloudTrail log.

This demonstrates how CloudTrail can be used to correlate AWS API activity with specific resources and encryption operations.

### Screenshot

![Image Name in CloudTrail](screenshots/12-Open%20log%20file%20contain%20image%20name.png)

---

# 👤 12. Manage KMS Key Users

The KMS key permissions were modified to demonstrate how access to a customer managed key can be controlled.

First, the current IAM user was removed from the **Key Users** list.

### Screenshot

![Remove Key User](screenshots/13-Remove%20key%20user.png)

The user was then added again to restore permission to use the key.

### Screenshot

![Add Key User](screenshots/14-%20Add%20key%20user.png)

This demonstrates that KMS provides granular control over which IAM users or roles can use a key for encryption and decryption operations.

---

# 🧠 Key Takeaways

This lab provided practical understanding of the relationship between **Amazon S3, AWS KMS, IAM, and CloudTrail**.

### Encryption

AWS KMS can provide centralized management of encryption keys used to protect data stored in AWS services.

### Access Control

Access to an encrypted S3 object depends on more than the S3 object permissions. The principal also needs the appropriate permissions to use the KMS key.

### Auditing

AWS CloudTrail provides visibility into API activity and can be used to investigate encryption-related operations.

### SSE-KMS

Server-side encryption with AWS KMS keys provides an additional layer of protection and requires authenticated access when interacting with encrypted objects.

### Key Management

KMS key administrators and key users can be managed independently, allowing more granular control over encryption key usage.

---

# 📁 Repository Structure

```text
AWS-KMS-Lab/
│
├── screenshots/
│   ├── 01-Create key.png
│   ├── 02-Create trail.png
│   ├── 03-Upload image file with encrypted key.png
│   ├── 03-Open image from consol.png
│   ├── 04-Access Denied.png
│   ├── 05-Deselect block public access.png
│   ├── 06-Successfully edited Object Ownership..png
│   ├── 07-Make public object.png
│   ├── 08- Request specific server side Encryption.png
│   ├── 09-Log file.png
│   ├── 11-Open log file contain keyID.png
│   ├── 12-Open log file contain image name.png
│   ├── 13-Remove key user.png
│   └── 14- Add key user.png
│
└── README.md
```

---

# ✅ Skills Demonstrated

* AWS Key Management Service (KMS)
* Symmetric Customer Managed Keys
* Amazon S3 Server-Side Encryption
* SSE-KMS
* S3 Object Permissions
* S3 Block Public Access
* S3 Object Ownership and ACLs
* AWS CloudTrail
* IAM Permissions
* Encryption Key Management
* CloudTrail Log Analysis
* AWS Security Best Practices

---

## 📌 Lab Summary

This project demonstrates a complete practical workflow for **encrypting an Amazon S3 object with AWS KMS, controlling access to the encryption key, examining S3 access behavior, and auditing related activity using CloudTrail**.

The screenshots included in this repository provide visual evidence of the major configuration and verification steps performed during the lab.
