# Architecture

## Overview

This project describes a simple architecture for **Row Level Security (RLS)** in a tabular model (Power BI / Analysis Services).

The goal is to control **which data each user can see**, using the natural filter flow of the model.

The architecture follows a common rule in dimensional models:

**Security starts in a dimension and flows down to fact tables.**

---

# Basic Model Structure

The model is organized as a hierarchy of dimensions that eventually connect to a fact table.

Example:
Hypergroup
↓
Profit Center
↓
WBS
↓
FactWVS

Explanation:

- **Hypergroup** → highest organizational level  
- **Profit Center** → financial unit  
- **WBS** → project or operational unit  
- **FactWVS** → table containing the metrics

---

# Relationship Flow

The model uses **one-to-many relationships**.
Dimension (1) → Fact (Many)

Example:
Hypergroup (1)
↓
Profit Center (Many)

Profit Center (1)
↓
WBS (Many)

WBS (1)
↓
FactWVS (Many)

This structure allows filters to move **from top to bottom**.

---

# Why This Matters for Security

Row Level Security works because filters travel through the model.

If a user is restricted to one Hypergroup:

Hypergroup = "Finance"


The filter automatically applies to:


Finance Hypergroup
↓
Finance Profit Centers
↓
Finance WBS
↓
Finance records in FactWVS


This means we only need to apply security **once**, at the dimension level.

---

# Example

Imagine the following data:

Hypergroup

Finance
Operations
Marketing


If a user only has access to **Finance**, the model will automatically filter:


Finance → Profit Centers → WBS → Fact data


The user will never see data from **Operations or Marketing**.

---

# Central Security Layer

To support multiple access rules, a **security table** can be added.

Example:

SecurityTable

UserEmail
AccessLevel
EntityKey


Example rows:


alice@company.com
 | Hypergroup | Finance
bob@company.com
 | ProfitCenter | PC100
admin@company.com
 | SuperUser | ALL


This table connects the **user identity** with the part of the model they are allowed to see.

---

# Key Idea

The architecture keeps security **simple and scalable**:

- Security starts in a **dimension**
- Filters move through **relationships**
- Fact tables are automatically restricted

This avoids complex logic and keeps the model easy to maintain.

Pequeño tip de ingeniero para tu repo, mae:
cuando subás esto, tu estructura debería verse así:

powerbi-rls-security-architecture
│
README.md
│
docs
│
architecture.md
rls-design.md
testing-rls.md
dax-patterns.md
