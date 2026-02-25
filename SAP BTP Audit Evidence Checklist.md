# SAP BTP Audit Evidence Checklist

This checklist helps auditors and compliance teams verify the implementation of identity, access, and governance controls for SAP BTP using Microsoft Entra ID, SAP IAS, and SAP IPS.

## Table of Contents
1. Federation & SSO
2. Group-Based Provisioning
3. RBAC Model & Automation
4. Admin Elevation (PIM)
5. Emergency Access (Break-Glass)
6. Access Reviews
7. Monitoring & Audit Logs
8. Compliance Alignment

---

## 1. Federation & SSO
- [ ] IAS configured as SAML broker with Entra ID
- [ ] Attribute mapping: loginName = userPrincipalName
- [ ] User creation at logon enabled

## 2. Group-Based Provisioning
- [ ] Entra Security Groups (SG-BTP-*) created
- [ ] IPS source/target configured for users, groups, memberships
- [ ] Group/user filters applied (aad.group.filter, etc.)
- [ ] User mapping: userPrincipalName → userName

## 3. RBAC Model & Automation
- [ ] Role collections created per environment (RC_<ENV>_<ACCESS>)
- [ ] Role collections mapped to IAS groups
- [ ] BTP CLI used for CI/CD deployment
- [ ] Git repository contains RBAC templates

## 4. Admin Elevation (PIM)
- [ ] PIM enabled for PROD admin groups
- [ ] Eligible assignments, approval, MFA, ticket ID required
- [ ] 2-hour max duration enforced

## 5. Emergency Access (Break-Glass)
- [ ] IAS local user (btp-emergency-admin) exists
- [ ] Emergency group and role collection mapped in PROD
- [ ] Credentials stored in Azure Key Vault, rotated every 90 days

## 6. Access Reviews
- [ ] Entra Access Reviews scheduled (monthly/quarterly/bi-annual)
- [ ] Auto-removal for non-responders enabled

## 7. Monitoring & Audit Logs
- [ ] BTP Audit Log Service enabled in all subaccounts
- [ ] Logs exported to Azure Log Analytics/Microsoft Sentinel
- [ ] Alerts configured for critical events

## 8. Compliance Alignment
- [ ] Controls mapped to ISO 27001, SOX, NIST Zero Trust

---

**Use this checklist during audits or compliance reviews to ensure all controls are in place and operating effectively.**
