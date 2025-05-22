# 🌐 Multi-Cloud Data Transfer: AWS S3 ➜ GCP Cloud Storage
![image](https://github.com/user-attachments/assets/0c144e4c-fa72-40d4-ba76-51109309b0dc)

This project demonstrates a **real-world multi-cloud data transfer pipeline**, where we securely transfer data between **Amazon S3** and **Google Cloud Storage** using **GCP's Storage Transfer Service**, **AWS IAM**, and **Amazon SQS** for real-time syncs.

## 🚀 Why This Project?

Engineering teams increasingly use **multi-cloud** strategies to:
- Improve **redundancy**
- Avoid **vendor lock-in**
- Optimize **cost-performance**

This project showcases how to **seamlessly move and synchronize data across AWS and GCP**, ensuring a reliable and secure storage backup solution.

---

## 🧰 Services Used

### ✅ AWS:
- **Amazon S3** — Object storage (source of our files)
- **AWS IAM** — Identity and access management for secure federation
- **Amazon SQS** *(planned extension)* — Real-time event-driven data syncs

### ✅ Google Cloud Platform (GCP):
- **Cloud Storage** — Object storage (destination)
- **Storage Transfer Service** — Transfers data between cloud services
- **Google IAM** — Secures transfer service authentication

---

## 🗂️ Project Breakdown

### 🔧 Part 1: Set Up AWS and GCP

#### ✅ Step 1: Create and Populate an S3 Bucket
- **Bucket name:** `johnlewis-data-transfer-source`
- Uploaded 1–3 files for initial transfer
<img width="1110" alt="S3 Objects" src="https://github.com/user-attachments/assets/25f4dacd-5599-4bc8-895e-0602b1788b91" />

#### ✅ Step 2: Create a GCP Account
- Signed up via [cloud.google.com](https://cloud.google.com)
- GCP free tier includes $300 credits & essential services like GCS
<img width="1108" alt="GCP Account" src="https://github.com/user-attachments/assets/138ffbc6-0552-4314-a3b4-3d987237232c" />

---

### 🔗 Part 2: Connect AWS to GCP

#### ✅ Step 3: Set Up Storage Transfer
- In GCP, created a new **Transfer Job**
- Source: `Amazon S3`
- Destination: `Google Cloud Storage`
- Schedule: **Batch** (for one-time migration)
<img width="1072" alt="Screenshot 2025-05-22 at 1 21 54 PM" src="https://github.com/user-attachments/assets/9718713d-218c-4eac-9ae6-9defa2b78049" />


#### ✅ Step 4: Configure Secure Access with IAM
- Created a **Custom IAM Role** in AWS
- Set a **custom trust policy** for **GCP identity federation**
- Attached **AmazonS3ReadOnlyAccess** to allow S3 read access

```json
{
"Version": "2012-10-17",
"Statement": [
 {
   "Effect": "Allow",
   "Principal": {
     "Federated": "accounts.google.com"
   },
   "Action": "sts:AssumeRoleWithWebIdentity",
   "Condition": {
     "StringEquals": {
       "accounts.google.com:sub": "YOUR_SUBJECT_ID"
     }
   }
 }
]
}
```

---

### 💾 Part 3: Run the Transfer Job

#### ✅ Step 5: Perform the Transfer
- Created destination bucket: `johnlewis-data-transfer-destination-gcp`
- Selected **Region** & **Standard** storage class
- Successfully transferred files from S3 ➜ GCP!

#### ✅ ✅ Verified the job:
- Checked **Storage Transfer logs**
- Verified files appear in **GCP Cloud Storage**
<img width="1104" alt="Transfer job done" src="https://github.com/user-attachments/assets/5df449e9-7dd6-4187-aeef-18172850f25e" />

---

## 🎯Selective Transfer using Manifest File
<img width="1685" alt="s3 last" src="https://github.com/user-attachments/assets/720c336b-b0f5-44c0-896c-0619047c459b" />

### Why Use a Manifest?
Manifest files let us transfer **only specific files** — useful for enterprise data migration and fine-tuned control.

#### 📄 Created a `manifest.csv`:
Created manifest.csv via terminal:
```
cd Desktop
touch manifest.csv
nano manifest.csv
```
<img width="567" alt="created manifest file" src="https://github.com/user-attachments/assets/05f15674-731c-4072-a0f9-2a842c862eb6" />

Uploaded `manifest.csv` to GCP Storage
Triggered a manifest-based selective transfer
<img width="844" alt="final mani" src="https://github.com/user-attachments/assets/fc87db9d-106f-4b92-a8e5-da654985530f" />
