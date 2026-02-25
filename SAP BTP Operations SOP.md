# SAP BTP Operations SOP (Standard Operating Procedures)

This SOP provides step-by-step operational procedures for managing SAP BTP identity, access, and governance using Microsoft Entra ID, SAP IAS, and SAP IPS.

## Table of Contents
1. Onboarding New Users
2. Managing Group Memberships
3. Role Collection Deployment (CI/CD)
4. Admin Elevation (PIM)
5. Emergency Access (Break-Glass)
6. Access Reviews
7. Monitoring & Audit

---

## 1. Onboarding New Users
- Add user to appropriate Entra Security Group (SG-BTP-...)
- IPS auto-provisions user to IAS and assigns group
- User logs in via SSO (Entra → IAS → BTP)

## 2. Managing Group Memberships
- Add/remove users in Entra Security Groups
- IPS syncs changes to IAS
- IAS group membership updates reflected in BTP role collections

## 3. Role Collection Deployment (CI/CD)
- Update RBAC templates in Git repository
- Pipeline triggers BTP CLI to deploy/assign role collections
- Validate assignments in BTP subaccounts

## 4. Admin Elevation (PIM)
- Eligible user requests activation for PROD admin group
- Approval, MFA, and ticket ID required
- Access granted for 2 hours, auto-revoked after

## 5. Emergency Access (Break-Glass)
- Use IAS local user (btp-emergency-admin) if federation fails
- Credentials stored in Azure Key Vault
- Rotate credentials every 90 days

## 6. Access Reviews
- Conduct periodic reviews in Entra (monthly/quarterly/bi-annual)
- Remove users who fail review; IPS syncs removal

## 7. Monitoring & Audit
- Review BTP Audit Log Service for privileged activity
- Export logs to Azure Log Analytics/Microsoft Sentinel
- Set up alerts for critical events (role assignment, break-glass login, etc.)

---

**Note:** All procedures align with least privilege, zero trust, and compliance requirements (ISO 27001, SOX).
