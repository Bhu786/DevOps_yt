[](./ec2_definition.png)
==============================
Haan 👍 **AWS me EC2 Snapshot backup ke liye hi use hota hai.**

### 📌 EC2 Snapshot kya hota hai?

**Snapshot ek backup copy hoti hai EC2 instance ke EBS volume ki.**
Ye **Amazon S3 me store hoti hai**, lekin aapko directly S3 bucket nahi dikhti.

---

### 🔹 Snapshot se kya kar sakte ho?

1️⃣ **Backup rakhna**
Agar EC2 crash ho jaye ya data delete ho jaye to snapshot se restore kar sakte ho.

2️⃣ **New volume create karna**
Snapshot se naya **EBS volume** bana sakte ho.

3️⃣ **New EC2 instance launch karna**
Snapshot → volume → instance attach karke.

4️⃣ **Disaster recovery**
Agar server fail ho gaya to snapshot se jaldi recovery ho jati hai.

---

### 🔧 Simple flow

```
EC2 Instance
     ↓
EBS Volume
     ↓
Snapshot (Backup)
     ↓
New Volume / Restore
```

---

### 🧠 Example

Maan lo EC2 me database hai.

* Aap snapshot lete ho
* Agar database corrupt ho jaye
* To snapshot se **purana stable version restore kar sakte ho**

---

### ⚡ Important points

* Snapshot **incremental backup** hota hai (sirf changed data store hota hai)
* Snapshot se **new EBS volume create hota hai**
* Automatic snapshot ke liye **AWS Backup / Lifecycle policies** use kar sakte ho

---

✅ **Short me:**
**EC2 Snapshot = EBS volume ka backup**

---

==========================
[](./ec2_types.png)
=================
## typ of ec2 connection to local 
[](ec2_connection.png)
=============================


[](iam.png)

## IMA ==> Permissions 
Aapke screenshot me **AWS IAM (Identity and Access Management)** ke 4 main parts dikh rahe hain:

* **User groups**
* **Users**
* **Roles**
* **Policies**

In sab ka relation samajhna bahut important hai. 👇

---

## IAM Structure (Simple Relation)

```
Policy → attach hoti hai → User / Group / Role
```

Policy me **permissions hoti hain** (kya allow hai / kya deny hai).

---

## 1️⃣ Users

👤 **Individual person ya application**

Example:

* `Rahul`
* `Developer1`

User login kar sakta hai AWS console me.

**Policy attach kar sakte ho directly user par.**

```
User → Policy
```

---

## 2️⃣ User Groups

👥 **Multiple users ka group**

Example:

* Developers
* Admins
* DevOps

Group par policy attach karo to **group ke sab users ko permission mil jati hai**.

```
Users → Group → Policy
```

Example:

```
Developer Group
   ├── User1
   ├── User2
   └── User3
```

Agar **S3 access policy** group par lagayi to sabko mil jayegi.

---

## 3️⃣ Roles

🎭 **Temporary permissions ke liye**

Roles normally use hote hain:

* EC2
* Lambda
* Cross account access

Example:

```
EC2 Instance → Assume Role → Access S3
```

Isme EC2 ko **access key nahi chahiye**.

Flow:

```
Policy → Role → EC2
```

---

## 4️⃣ Policies

📜 **Permissions ka JSON document**

Example policy:

```json
{
 "Effect": "Allow",
 "Action": "s3:ListBucket",
 "Resource": "*"
}
```

Iska matlab:

* S3 bucket list karne ki permission.

Policy attach ho sakti hai:

* User
* Group
* Role

---

## 🔑 Real AWS Example

Agar EC2 ko S3 access dena hai:

```
Policy (S3Access)
        ↓
      Role
        ↓
      EC2 Instance
```

EC2 automatically S3 access kar lega.

---

## 📊 Final Relation Diagram

```
            Policy
           /  |  \
         User Group Role
           |    |
         Login  EC2/Lambda
```

---

✅ **Short answer (jo aap pooch rahe the):**

> **Policy banake Role par lagate hain, phir Role ko EC2 ya service use karti hai.**

---


=====================

[](./s3.png)

=====================
[](./route_53.png)
[](./most_asked_part.png)

====================
[](./Cloud_Trail.png)

=====================
[](./Cloud_watch.png)
| Feature   | Logs        | Metrics                | Traces           |
| --------- | ----------- | ---------------------- | ---------------- |
| Data Type | Text events | Numbers                | Request path     |
| Purpose   | Debugging   | Performance monitoring | Request tracking |
| Example   | Error log   | CPU 80%                | Request flow     |
================================
[](./Autoscaling.png)
==================================
ECS ===> JUST LIKE DOCKER AWS APNA BNA LEYA HAI BAS 
===============================
[](./lambda.png)
## max compute tme ==> 
AWS **AWS Lambda** me **function execution ka maximum time limit hota hai**.

## ⏱️ Lambda Maximum Execution Time

| Type         | Time                         |
| ------------ | ---------------------------- |
| Minimum time | **1 second**                 |
| Maximum time | **15 minutes (900 seconds)** |

Matlab:

```text
1 sec  ≤  Lambda execution time  ≤  900 sec
```

Agar function **15 minutes se zyada run kare**, to Lambda automatically **timeout error** de deta hai.

---

## 📊 Example

Agar aapne Lambda function set kiya:

```text
Timeout = 10 seconds
```

Aur code run hone me **12 seconds lag gaye**, to result:

```text
Task timed out error
```

---

## 🔧 Timeout kaha set karte hain?

AWS Console me:

```text
Lambda → Configuration → General configuration → Timeout
```

Yahan aap **1 second se 15 minutes** tak set kar sakte ho.

---

## 🧠 Lambda long processing ke liye kya karte hain?

Agar job **15 minutes se zyada** ho:

Use services like:

* AWS Step Functions
* Amazon EC2
* AWS Batch

---

✅ **Interview answer (short)**

> AWS Lambda ka **maximum execution time 15 minutes (900 seconds)** hota hai.

---


=======================
[](./aws_shield.png)
=========================
[](./AWS_WAF.png)
==========================
