# 🐘 AWS RDS Backup & Restore Lab — Snapshots, Restore, and Instance Cleanup

In this lab, I learned how to **create automated/manual RDS snapshots, restore a new database instance from a snapshot, view RDS metadata, and safely delete RDS instances**.

---

## 📋 Lab Overview

**Goal:**

* Launch a MariaDB RDS instance using Easy Create
* Understand RDS as a fully managed relational database service
* Take a **manual snapshot** of an RDS instance
* Restore a **new RDS instance** from that snapshot
* Delete both the original DB and the restored DB safely

**Learning Outcomes:**

* Use Easy Create to deploy a MariaDB RDS instance
* Understand the benefits of RDS managed automation
* Create manual DB snapshots
* Restore DB instances from snapshots
* Identify RDS metadata such as endpoint and resource ID
* Delete RDS workloads while disabling final snapshots and backup retention

---

## 🛠 Step-by-Step Journey

### **Step 1 — Log Into AWS Console**

Logged into the AWS Management Console using the provided lab credentials.
Region: **US East (N. Virginia)**.

---

### **Step 2 — Create a MariaDB RDS Instance (Easy Create)**

Navigated to: **RDS → Create Database → Easy Create**

**Instance Configuration:**

| Setting         | Value                 |
| --------------- | --------------------- |
| Create Method   | Easy Create           |
| Engine          | MariaDB               |
| Instance Size   | Free Tier             |
| DB Identifier   | `my-mariadb-instance` |
| Master Username | `admin`               |
| Password        | Auto-generated        |
| Other Settings  | Left as default       |

Clicked **Create Database**.

---

### **Step 3 — Understand RDS Managed Automation**

The lab reviewed what AWS RDS automatically manages:

✔ Installation
✔ Storage provisioning
✔ Patching (OS + DB engine)
✔ Backup + restore
✔ Snapshot management

---

### **Step 4 — Confirm Snapshot Backup Behavior**

RDS snapshots are **incremental**.

Correct answer:
✅ RDS automation determines which blocks need to be backed up during snapshot creation.

---

### **Step 5 — Wait for RDS Instance to Become Available**

Status progressed from **Creating → Backing-up → Available**.

Once available → moved to the next step.

---

### **Step 6 — Retrieve Connection Details**

Clicked on the instance → viewed metadata.

| Property    | Value                                             |
| ----------- | ------------------------------------------------- |
| Endpoint    | `my-mariadb-instance.us-east-1.rds.amazonaws.com` |
| Resource ID | e.g. `db-sbxxxxxxxxxxxxx`                         |

Recorded these for the lab questions.

---

### **Step 7 — Take a Manual Snapshot**

Navigated to:

**RDS → Databases → my-mariadb-instance → Actions → Take Snapshot**

**Snapshot Details:**

| Setting       | Value                 |
| ------------- | --------------------- |
| Snapshot Type | DB Instance           |
| DB Instance   | `my-mariadb-instance` |
| Snapshot Name | `my-mariadb-snapshot` |

Clicked **Take Snapshot**.

Snapshot entered **Creating** state → after a few minutes changed to:

```
Available
```

---

### **Step 8 — Restore a DB Instance from the Snapshot**

From **RDS → Snapshots → my-mariadb-snapshot → Actions → Restore Snapshot**

**Restore Configuration:**

| Setting        | Value                      |
| -------------- | -------------------------- |
| DB Identifier  | `my-mariadb-from-snapshot` |
| Instance Class | Burstable (db.t4g.micro)   |
| Storage Type   | GP2                        |
| Storage        | 20 GB                      |
| Other Settings | Default                    |

Clicked **Restore DB Instance**.

After a short wait, the restored DB reached:

```
Available
```

---

### **Step 9 — Delete Both RDS Instances**

Final task: delete the **original DB instance** and the **restored DB instance**.

Deletion settings for both:

* ❌ Uncheck: Create final snapshot
* ❌ Uncheck: Retain automated backups
* ☑️ Acknowledge deletion warning
* Type: `delete me`

Both instances successfully deleted.

---

## 💡 Notes / Tips

* **Easy Create** is perfect for quick labs — it autoconfigures networking and defaults.
* RDS **snapshots are incremental**, meaning faster and storage-efficient.
* Restoring from a snapshot creates a **new DB instance** — it does *not* overwrite the original.
* For production systems, always keep automated backups enabled.
* Deleting an RDS instance without snapshots is final — use carefully.

---

## 📌 Lab Summary

| Step                      | Status | Key Takeaways                               |
| ------------------------- | ------ | ------------------------------------------- |
| Create MariaDB instance   | ✅      | Easy Create simplifies setup                |
| Understand RDS automation | ✅      | AWS handles backups, patching, provisioning |
| Take snapshot             | ✅      | Snapshot created + available                |
| Restore from snapshot     | ✅      | New DB instance launched                    |
| Delete both instances     | ✅      | No snapshots retained, lab cleaned up       |

---

## 🔗 References

* [Amazon RDS Documentation](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html)
* [RDS Snapshots](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_CreateSnapshot.html)
* [Restoring from Snapshots](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_RestoreFromSnapshot.html)
