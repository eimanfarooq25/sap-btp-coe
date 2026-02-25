# SAP BTP Identity & Access Governance Architecture Runbook

This runbook provides a logical and technical architecture overview for implementing Microsoft Entra ID–driven identity and access governance for SAP BTP.

## Table of Contents
1. Purpose
2. Logical Architecture
3. Naming Standards
4. Federation Configuration
5. Auto Provisioning (IPS)
6. BTP RBAC Model
7. RBAC Automation
8. Admin JIT Access
9. Conditional Access
10. Break-Glass Access
11. Monitoring
12. Access Reviews
13. Operations Checklist

---

## 1. Purpose
Defines the architecture, governance model, automation, admin elevation, emergency recovery, and monitoring for SAP BTP platform services across DEV, QA, and PROD.

## 2. Logical Architecture
- **Identity Flow:**
  - User → Microsoft Entra ID → SAP IAS → SAP BTP Subaccount → Role Collection → Service Role
- **Authorization Flow:**
  - Entra Group (SG-BTP-*) → IPS Provisioning → IAS Group → Mapped to Role Collection → Access to BTP services

## 3. Naming Standards
- **Entra Security Groups:** SG-BTP-<ENV>-<ACCESS>
- **Role Collections:** RC_<ENV>_<ACCESS>

## 4. Federation Configuration
- Configure IAS ↔ Entra trust (SAML 2.0)
- Map loginName = userPrincipalName
- Enable user creation at logon

## 5. Auto Provisioning (IPS)
- Source: Microsoft Azure AD (users, groups, memberships)
- Filter: Only SG-BTP-* groups
- User mapping: userPrincipalName → userName

## 6. BTP RBAC Model
- Trust IAS in each subaccount
- Create and map role collections to IAS groups
- Assign BTP service roles (e.g., Developer, Launchpad_User)

## 7. RBAC Automation
- Use BTP CLI in CI/CD pipeline
- Deploy and assign role collections via code
- Map IAS groups to role collections

## 8. Admin JIT Access
- Enable PIM for PROD groups in Entra
- Assign eligible membership, require MFA, approval, ticket ID, 2h max

## 9. Conditional Access
- Apply to SAP IAS Enterprise App
- Require MFA, compliant device, low risk login

## 10. Break-Glass Access
- Create IAS local user (btp-emergency-admin)
- Map to emergency role collection
- Store credentials securely, rotate every 90 days

## 11. Monitoring
- Enable BTP Audit Log Service
- Track role assignments, admin changes, service instance creation
- Export logs to SIEM

## 12. Access Reviews
- Configure periodic reviews in Entra (monthly/quarterly/bi-annual by environment)
- Auto-remove non-responders

## 13. Operations Checklist
| Control              | Status   |
|----------------------|----------|
| IAS Federation       | Enabled  |
| IPS Filtering        | Enabled  |
| RBAC Automation      | Active   |
| PIM JIT Access       | Enabled  |
| Conditional Access   | Active   |
| Break-Glass          | Tested   |
| Audit Logs           | Exported |
| Access Reviews       | Scheduled|

---

**Compliance:** ISO 27001, SOX, NIST Zero Trust, Least Privilege
