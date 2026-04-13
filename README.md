# TechCorp Security Model POC — Salesforce Admin

A hands-on Salesforce project demonstrating the complete Security Model using a fictional company (TechCorp) with a real sales hierarchy, custom objects, and layered access control.

---

## 📌 What This Project Covers

| Layer | Implementation |
|-------|---------------|
| Custom Object | `Deal__c` with Amount, Stage, Region fields |
| Lightning App | TechCorp Deals — custom navigation app |
| Role Hierarchy | VP → Regional Managers → Sales Reps |
| User Management | 5 users assigned to roles and custom profile |
| OWD | Set to Private — no default cross-user visibility |
| Sharing Rule | North deals shared read-only with South team |
| Permission Set | `Deal_Full_Visibility` — grants View All to specific users |

---

## 🏗️ Org Structure

```
VP Sales (Alice)
├── Regional Manager - North (Bob)
│   └── Sales Rep - North (Dave) → owns Deal North 1, Deal North 2
└── Regional Manager - South (Carol)
    └── Sales Rep - South (Eve) → owns Deal South 1, Deal South 2
```

---

## 🔐 Security Model Explained

### 1. Object-Level Security (Profile)
- Custom Profile: `TechCorp Sales Rep`
- Permissions: Read, Create, Edit on `Deal__c`
- No View All / Modify All

### 2. OWD (Organization-Wide Defaults)
- `Deal__c` OWD set to **Private**
- By default, users can only see records they own

### 3. Role Hierarchy
- Managers automatically inherit visibility of their subordinates' records
- Bob (North Manager) → sees Dave's deals
- Carol (South Manager) → sees Eve's deals
- Alice (VP) → sees all deals

### 4. Sharing Rule
- Rule: `North to South Read Access`
- Eastern Sales Team records shared with Western Sales Team
- Access Level: Read Only

### 5. Permission Set
- Name: `Deal_Full_Visibility`
- Grants: View All Records on `Deal__c`
- Assigned to: Eve TechCorp
- Result: Eve can see all deals regardless of OWD

---

## 📁 Project Structure

```
force-app/
└── main/
    └── default/
        ├── objects/
        │   └── Deal__c/
        │       ├── Deal__c.object-meta.xml
        │       ├── fields/
        │       │   ├── Amount__c.field-meta.xml
        │       │   ├── Stage__c.field-meta.xml
        │       │   └── Region__c.field-meta.xml
        │       └── listViews/
        │           └── All.listView-meta.xml
        ├── profiles/
        │   └── TechCorp Sales Rep.profile-meta.xml
        └── permissionsets/
            └── Deal_Full_Visibility.permissionset-meta.xml
```

---

## 🚀 How to Deploy to Your Org

### Prerequisites
- Salesforce CLI installed
- VS Code with Salesforce Extension Pack

### Steps

```bash
# Clone the repo
git clone https://github.com/hritishmahajan/TechCorp-Security-Model-POC---Salesforce-Admin.git
cd TechCorp-Security-Model-POC---Salesforce-Admin

# Authorize your org
sf org login web --alias myorg

# Deploy metadata
sf project deploy start --target-org myorg
```

---

## 🎯 Key Concepts Demonstrated

- **OWD** controls the baseline access floor for all records
- **Role Hierarchy** gives managers automatic access to subordinates' records
- **Sharing Rules** create specific exceptions to OWD for cross-team access
- **Permission Sets** are additive — they only grant, never restrict
- **Profiles** control object-level and field-level access

---

## 👨‍💻 Author

**Hritish Mahajan**  
B.E.Tech Student @ Thapar University | Technical Associate @ Caelius Consulting  
[GitHub](https://github.com/hritishmahajan)
