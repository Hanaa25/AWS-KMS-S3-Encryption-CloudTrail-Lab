# AWS KMS, S3 Encryption & CloudTrail Lab

**Keywords:** `KMS` • `CloudTrail` • `S3`

## 📌 Project Overview

This hands-on AWS lab demonstrates how **AWS Key Management Service (KMS)** can be used to encrypt objects stored in **Amazon S3**, while **AWS CloudTrail** provides visibility into related API activity for monitoring and auditing.

The project focuses on the practical relationship between **encryption, access control, key permissions, and audit logging** in AWS.

It also includes hands-on testing of access behavior for an S3 object protected with **SSE-KMS**, followed by CloudTrail log analysis to identify the KMS key and related object activity.

---

## 🎯 Objectives

The main objectives of this project were to:

* Create a customer-managed symmetric KMS key.
* Configure KMS key administrators and key users.
* Encrypt an S3 object using **SSE-KMS**.
* Test access to an encrypted S3 object.
* Investigate an **Access Denied** scenario.
* Configure an AWS CloudTrail trail.
* Analyze CloudTrail logs related to the encrypted object.
* Identify the KMS Key ID from CloudTrail events.
* Identify the S3 object involved in the recorded activity.
* Practice adding and removing a KMS key user.

---

## ☁️ AWS Services

| Service            | Purpose                                    |
| ------------------ | ------------------------------------------ |
| **AWS KMS**        | Encryption key creation and key management |
| **Amazon S3**      | Storage of the encrypted object            |
| **AWS CloudTrail** | API activity monitoring and auditing       |
| **IAM**            | Identity and permission management         |

---

## 🔄 Project Workflow

```text
Create KMS Key
      │
      ▼
Configure Key User
      │
      ▼
Upload Object to S3
      │
      ▼
Encrypt Object with SSE-KMS
      │
      ▼
Test Object Access
      │
      ▼
Investigate Access Behavior
      │
      ▼
Monitor Activity with CloudTrail
      │
      ▼
Analyze CloudTrail Logs
      │
      ▼
Manage KMS Key Users
```

---

# 🧪 Implementation

## 1. Create the KMS Key

A **symmetric customer-managed KMS key** was created through the AWS Management Console.

### Configuration

* **Key Type:** Symmetric
* **Alias:** `myFirstKey`
* **Description:** KMS Key for S3 data
* **Key Administrator:** Lab IAM user
* **Key User:** Lab IAM user

The generated **Key ID** was recorded for later verification through CloudTrail.

### Evidence

![Create KMS Key](screenshots/01-Create%20key.png)

---

## 2. Configure CloudTrail

An AWS CloudTrail trail named `myTrail` was configured to record AWS activity.

The configuration included relevant management and data events, together with CloudTrail Insights monitoring.

### Evidence

![Create CloudTrail](screenshots/02-Create%20trail.png)

---

## 3. Upload and Encrypt an S3 Object

An image was uploaded to Amazon S3 and protected using **Server-Side Encryption with AWS KMS (SSE-KMS)**.

The customer-managed key `myFirstKey` was selected for encryption.

### Evidence

![Upload Encrypted Image](screenshots/03-%20Upload%20image%20file%20with%20encrypted%20key.png)

---

## 4. Access the Encrypted Object

The encrypted image was opened through the Amazon S3 console to verify access to the protected object.

### Evidence

![Open Encrypted Image](screenshots/03-Open%20image%20from%20consol.png)

---

## 5. Investigate Access Denied

An **Access Denied** response was encountered during the access test.

This provided a practical demonstration that access to encrypted S3 data can involve multiple layers of authorization, including S3 and KMS permissions.

### Evidence

![Access Denied](screenshots/04-Access%20Denied.png)

---

## 6. Review S3 Block Public Access

As part of the lab experiment, the S3 **Block Public Access** configuration was modified to investigate object access behavior.

### Evidence

![Deselect Block Public Access](screenshots/05-Deselect%20block%20public%20access.png)

> **Note:** This configuration change was performed for educational testing and is not intended as a production security recommendation.

---

## 7. Configure Object Ownership

The S3 **Object Ownership** configuration was modified as required for the access-control experiment.

### Evidence

![Object Ownership](screenshots/06-Successfully%20edited%20Object%20Ownership..png)

---

## 8. Test Public Object Access

The S3 object was configured for public access as part of the experiment.

The purpose was to examine whether S3-level public access would be sufficient to retrieve an object protected by SSE-KMS.

### Evidence

![Make Public Object](screenshots/07-Make%20public%20object.png)

---

## 9. Verify SSE-KMS Access Behavior

The server-side encryption configuration was reviewed to verify the use of the customer-managed KMS key.

This experiment demonstrated an important security concept:

> **S3 access permissions and KMS authorization are separate layers of control.**

Changing S3 access settings does not remove the authorization requirements associated with the KMS key.

### Evidence

![Server Side Encryption](screenshots/08-%20Request%20specific%20server%20side%20Encryption.png)

---

# 🔍 CloudTrail Monitoring & Log Analysis

## 10. Examine CloudTrail Logs

CloudTrail logs were reviewed to investigate API activity associated with the encrypted S3 object.

The recorded events provided visibility into AWS operations and supported the analysis of the encryption workflow.

### Evidence

![CloudTrail Log](screenshots/09-Log%20file.png)

---

## 11. Identify the KMS Key ID

The CloudTrail JSON event was examined to identify the **KMS Key ID** associated with the encryption activity.

### Evidence

![KMS Key ID](screenshots/11-Open%20log%20file%20contain%20keyID.png)

---

## 12. Identify the S3 Object

The CloudTrail event was further examined to identify the image/object associated with the recorded activity.

### Evidence

![S3 Object Name](screenshots/12-Open%20log%20file%20contain%20image%20name.png)

---

# 👤 KMS Key User Management

## 13. Remove the KMS Key User

The configured KMS key user was removed to demonstrate practical key-user permission management.

### Evidence

![Remove Key User](screenshots/13-Remove%20key%20user.png)

---

## 14. Add the KMS Key User

The user was subsequently added again as a KMS key user.

This demonstrated the practical management of identities authorized to use the encryption key.

### Evidence

![Add Key User](screenshots/14-%20Add%20key%20user.png)

---

# 🔐 Key Security Insights

### Encryption and Authorization

SSE-KMS provides encryption for the S3 object, while authorization to use the associated KMS key remains an additional access-control requirement.

### S3 and KMS Permissions

Access to an S3 object and permission to use a KMS key are separate controls. Troubleshooting encrypted-object access therefore requires considering both layers.

### Public Access Does Not Remove KMS Controls

The lab demonstrated that modifying S3 public-access settings does not eliminate the authorization requirements associated with KMS encryption.

### CloudTrail Auditing

CloudTrail provides visibility into AWS API activity and can be used to investigate events associated with resources and encryption operations.

### Key User Management

KMS supports controlled management of users who are authorized to use an encryption key, helping separate key administration from key usage.

---

# 🧠 Skills Demonstrated

### AWS Cloud

* AWS Key Management Service (KMS)
* Amazon S3
* AWS CloudTrail
* AWS IAM

### Cloud Security

* Data encryption at rest
* SSE-KMS
* Access control
* KMS key management
* Audit logging

### Practical Technical Skills

* AWS Management Console
* AWS resource configuration
* Permission troubleshooting
* CloudTrail JSON log analysis
* Encryption configuration
* KMS user management

---

# ⚠️ Security Considerations

Some S3 access settings were intentionally modified during this lab to demonstrate access-control behavior.

These changes were performed strictly for **hands-on learning and testing**.

For production environments, access to sensitive S3 data should be carefully controlled, KMS permissions should follow the **principle of least privilege**, and public access should only be enabled when explicitly required.

---

# 📁 Repository Structure

```text
AWS-KMS-S3-Encryption-CloudTrail-Lab/
│
├── screenshots/
│   ├── 01-Create key.png
│   ├── 02-Create trail.png
│   ├── 03- Upload image file with encrypted key.png
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

# ✅ Project Outcome

Successfully completed a hands-on AWS security workflow covering:

**KMS Key Creation → S3 Encryption → Access Testing → Permission Analysis → CloudTrail Monitoring → Log Analysis → KMS User Management**

The project demonstrates practical understanding of how **KMS, S3, CloudTrail, and IAM** work together to protect, control, and audit encrypted data in AWS.

---

## 📌 Conclusion

This project provided practical experience with AWS encryption and cloud security controls by combining **KMS-based encryption, S3 storage, access management, and CloudTrail auditing**.

It demonstrates not only the configuration of AWS services, but also the ability to **test access behavior, investigate authorization issues, and analyze audit logs** to understand what is happening inside an AWS environment.
