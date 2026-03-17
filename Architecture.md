# Architecture

## Overview

This project describes an architecture for **Row Level Security (RLS)** in a tabular model (Power BI / Analysis Services). The goal is to control **which data each user can see** using the natural filter flow of the model.

**Security starts in a dimension and flows down to fact tables.**

---

## Model Structure and Flow

The model uses a hierarchy of dimensions connected to multiple fact tables. And uses **one-to-many relationships**:

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


This allows filters to move **from top to bottom** through the model.

---

## Why This Matters for Security

RLS works because filters travel through relationships.

Example rule:
Hypergroup = "Finance"

This automatically filters:

Finance Hypergroup
↓
Finance Profit Centers
↓
Finance WBS
↓
Finance records in FactWVS


Security only needs to be applied **once at the dimension level**.

---

## Example

Example Hypergroups:
Finance
Operations
Marketing


If a user only has access to **Finance**, the model will filter:
Finance → Profit Centers → WBS → Fact data

The user will not see data from **Operations** or **Marketing**.

---

## Central Security Layer

A **security table** can connect users with the data they can access.

Example:
SecurityTable

UserEmail
AccessLevel
EntityKey


Example rows:
Alpha@company.com
 | Hypergroup | Finance
Beta@company.com
 | ProfitCenter | PC100
Gamma@company.com
 | SuperUser | ALL


This links **users** with the parts of the model they are allowed to see.

---

## Key Idea

The architecture keeps security simple and scalable:

- Security starts in a **dimension**
- Filters move through **relationships**
- Fact tables are automatically restricted
