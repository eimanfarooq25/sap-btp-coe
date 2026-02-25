# SAP BTP Identity & Access Governance Implementation Guide

This guide provides a step-by-step playbook for implementing Microsoft Entra ID–driven identity, access governance, RBAC automation, monitoring, and emergency access for SAP BTP.

## Table of Contents
1. Identity Federation (SSO)
2. Group-Based Auto Provisioning
3. Subaccount RBAC Model
4. RBAC Automation (CI/CD)
5. Access Governance
6. Admin JIT Access (PIM)
7. Emergency Break-Glass
8. Audit & Monitoring

---

## 1. Identity Federation (SSO)
**Goal:** Enable SSO between Microsoft Entra ID and SAP BTP via SAP IAS.

- Configure SAP IAS as the authentication broker.
- Add Microsoft Entra ID as a corporate identity provider (SAML 2.0).
- Upload Entra metadata XML.
- Map attributes:
  - `user.userprincipalname` → `loginName` (critical)
  - `givenName` → `firstName`
  - `surname` → `lastName`
  - `mail` → `email`
- Enable "User Creation at Logon".

## 2. Group-Based Auto Provisioning
**Goal:** Automate user/group provisioning from Entra ID to SAP IAS.

- Create SAP platform groups in Entra (SG-BTP-...)
- Configure SAP IPS source (Azure AD) and target (IAS)
- Enable users, groups, and group memberships
- Filter only SAP groups (aad.group.filter, etc.)
- Map userPrincipalName to userName
- Run provisioning job and validate in IAS

## 3. Subaccount RBAC Model
**Goal:** Enforce least privilege and environment isolation.

- Trust IAS in each BTP subaccount
- Create role collections (RC_<ENV>_<ACCESS>)
- Assign BTP service roles to collections
- Map role collections to IAS groups

## 4. RBAC Automation (CI/CD)
**Goal:** Deploy and manage RBAC via code.

- Store RBAC templates in Git (per environment)
- Install BTP CLI in pipeline
- Deploy role collections and assign roles via CLI
- Map IAS groups to role collections
- Trigger on Git commit

## 5. Access Governance
**Goal:** Enforce periodic access reviews and conditional access.

- Configure Entra Access Reviews (frequency by environment)
- Enable auto-apply and removal for non-responders
- Apply conditional access (MFA, device compliance, low risk)

## 6. Admin JIT Access (PIM)
**Goal:** Just-in-time admin elevation for PROD.

- Enable PIM for PROD groups
- Assign eligible (not active) membership
- Require approval, MFA, ticket ID, 2h duration

## 7. Emergency Break-Glass
**Goal:** Ensure emergency access if federation fails.

- Create IAS local user (btp-emergency-admin)
- Create IAS emergency group and role collection
- Map group in PROD, store credentials in Azure Key Vault
- Rotate credentials every 90 days

## 8. Audit & Monitoring
**Goal:** Monitor and alert on privileged activity.

- Enable BTP Audit Log Service in each subaccount
- Export logs to Azure Log Analytics/Microsoft Sentinel
- Trigger alerts for critical events (role assignment, break-glass login, etc.)

---

**Final State:**
- Entra-driven SAP BTP RBAC
- DEV/QA/PROD isolation
- CI/CD role deployment
- JIT admin access
- Conditional SAP login
- Access reviews
- Break-glass recovery
- Runtime admin monitoring
- End-to-end audit trail

_Aligned to Least Privilege, Zero Trust, ISO 27001, SOX controls._
