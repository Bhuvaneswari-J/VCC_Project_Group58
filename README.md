# VCC_Project_Group58
# Authors
Nikita Krishna-M23AID026-Designed and developed system to publish unauthorised login attempts to GCP VM.

Aparna Pundir-M23AID006-Designed and developed system to push unauthorized login attempt messages from pub/sub to BigQuery.

Bhuvaneswari J-M23AID053-Designed and developed K means ML model on Bigquery data.

# Insider Threat Detection System on GCP

This project simulates **malicious insider activity** such as unauthorized VM logins and suspicious data transfers, and uses **Google Cloud Platform (GCP)** to detect, log, analyze, and classify these behaviors.

# Colab file with python script and result

https://colab.research.google.com/drive/1d8JR2i8_R_3QqnD3QU9rMO0KzuS2syXk?usp=sharing

# Project Presentation ppt link

https://docs.google.com/presentation/d/1Pt4oMaNQesHlOFBdo6hEax7hU6F3p9QkMYgEBz1Yhzw/edit?usp=sharing

# Project Demo video link

https://drive.google.com/file/d/1fVVt9xKGeNu5yznakEuxQZWF3QktzY4A/view?usp=drivesdk

---
# Architecture
![image](https://github.com/user-attachments/assets/2adcec54-dcae-4e01-92f5-aabe5ce79007)


## Features

- ✅ Creates a VM on GCP using Python
- ✅ Generates and uploads SSH keys for remote login simulation
- ✅ Publishes login attempts and data transfer events to **Pub/Sub**
- ✅ Streams events to **BigQuery**
- ✅ Runs **KMeans clustering in BigQuery ML** to detect anomalies
- ✅ Visual monitoring with logs and query results

---

## Tech Stack

- **Google Colab + Python**
- **GCP Services**: Compute Engine, IAM, Pub/Sub, BigQuery, BigQuery ML, Cloud Storage
- **Libraries**: `google-cloud-pubsub`, `google-api-python-client`, `paramiko`, `bigquery`, `threading`, `datetime`

---

## Setup Instructions

### 1. Prerequisites

- A GCP Project with billing enabled.
- Enable these APIs:
  - Compute Engine API
  - IAM API
  - Pub/Sub API
  - BigQuery API
- Create or use a `default` VPC with SSH (TCP 22) allowed.

### 2. Authentication & Key Setup

- The script creates a GCP **Service Account** and assigns it the required roles.
- It then generates:
  - A **service account key** (`insider_key.json`)
  - An **SSH key pair** for VM login simulation (`insider_ssh_key`, `insider_ssh_key.pub`)

---

## What the Script Does

### 🔹 Infrastructure Setup

1. Creates a service account with required IAM roles.
2. Enables Compute, IAM, and Pub/Sub APIs.
3. Creates a **Debian-based e2-micro VM**.
4. Generates and uploads an SSH key to the VM.

### 🔹 Event Simulation

- Simulates **SSH login attempts** using `paramiko` for users like:
  - `bhuvna`, `nikita`, `aparna`, `admin`, `unknown_user`, etc.
- Simulates **data transfer events** with source & destination info.

### 🔹 Event Publishing

- Publishes all events (logins & transfers) to a **Pub/Sub topic** (`login-events`)
- Creates a **Pub/Sub subscription** to listen to and process messages.

### 🔹 Data Logging

- Inserts processed events into a **BigQuery table**: `secure_data.login_events`
- Creates a **feature table** and applies **KMeans clustering** using BigQuery ML.

---

## How to Simulate Behavior

1. Run the script cells sequentially in **Google Colab**.
2. Login attempts and simulated data transfers will be published.
3. BigQuery results can be verified using the included SQL or by querying directly.

---

## ML Model for Anomaly Detection

1. Creates numeric features:
   - `role_num`, `status_num`, `timestamp_unix`
2. Trains a **KMeans model** on these features.
3. Outputs results to `login_events_clustered` with assigned clusters.

---

## Resources Created

| Resource Type           | Name                       |
|------------------------|----------------------------|
| VM                     | `test-vm-insider`          |
| Pub/Sub Topic          | `login-events`             |
| Pub/Sub Subscription   | `login-events-sub`         |
| BigQuery Dataset       | `secure_data`              |
| BigQuery Table         | `login_events`             |
| Cloud Storage Bucket   | `save-secure-data`         |
| BigQuery ML Model      | `login_kmeans_model`       |
| Feature Table          | `login_events_features`    |
| Clustered Output Table | `login_events_clustered`   |

---

## IAM Roles Required

Ensure the service account has:
- `roles/pubsub.publisher`
- `roles/logging.logWriter`
- `roles/compute.admin`
- `roles/bigquery.admin`

---

## Sample Event Format

### 🔸 Login Event

```json
{
  "event_type": "login",
  "user": "admin",
  "timestamp": "2025-04-13T20:00:00Z",
  "status": "success",
  "role": "admin"
}
```
### 🔸 Data Transfer Event

```json
{
  "event_type": "data_transfer",
  "local_address": "192.168.1.10",
  "remote_address": "unknown.ip.address",
  "timestamp": 1713027600.0
}
```
## Future Improvements


Implement more sophisticated clustering algorithms like DBSCAN
or Spectral Clustering to detect irregular patterns that K-Means
might miss due to its assumptions on data distribution.


Extend the Pub/Sub-based pipeline to trigger real-time alerts
(emails, Slack notifications, etc.) when an anomaly is detected.


With labeled data (malicious vs. legitimate activity), supervised
models such as Random Forest or SVM can be trained for better
precision in detecting threats.


---

## Author

This project was developed as part of an academic requirement for the subject **Virtualization and Cloud Computing**.  
It demonstrates the use of GCP infrastructure and services to design a real-time insider threat detection pipeline leveraging:

- **Cloud-native event-driven architecture**
- **Automated VM management and login simulation**
- **Scalable analytics with BigQuery and BigQuery ML**

For academic or collaboration inquiries, please contact the contributor.
