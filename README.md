# 🔐 Keycloak IAM Homelab (SSO, MFA, LDAP, RBAC)

## 📌 Overview
This project demonstrates enterprise Identity and Access Management (IAM) concepts using Keycloak as the Identity Provider (IdP) and Grafana as a relying application.

It simulates real-world authentication and authorization flows including:

- Single Sign-On (SSO) using OpenID Connect (OIDC)
- Multi-Factor Authentication (MFA) with TOTP
- LDAP Federation (external identity source)
- Role-Based Access Control (RBAC)
- Token-based API authentication using JWT

---

## 🧱 Architecture

![Architecture Diagram](architecture/architecture-diagram.png)

User → Browser → Keycloak (IdP) → Grafana (App)
                         ↕
                      OpenLDAP

---

## 🛠️ Tech Stack

- Keycloak
- Grafana
- LLDAP 
- Docker / Docker Compose
- OpenID Connect (OIDC)

---

## 🔐 Keycloak Configuration

- Realm: corp
- Roles: admin, user, viewer
- Users:
  - admin1 → admin
  - user1 → user

---

## 🔑 Grafana SSO Config (Environment)

GF_AUTH_GENERIC_OAUTH_ENABLED=true
GF_AUTH_GENERIC_OAUTH_NAME=Keycloak
GF_AUTH_GENERIC_OAUTH_ALLOW_SIGN_UP=true
GF_AUTH_GENERIC_OAUTH_CLIENT_ID=grafana
GF_AUTH_GENERIC_OAUTH_CLIENT_SECRET=YOUR_CLIENT_SECRET
GF_AUTH_GENERIC_OAUTH_SCOPES=openid profile email
GF_AUTH_GENERIC_OAUTH_AUTH_URL=http://localhost:8080/realms/corp/protocol/openid-connect/auth
GF_AUTH_GENERIC_OAUTH_TOKEN_URL=http://localhost:8080/realms/corp/protocol/openid-connect/token
GF_AUTH_GENERIC_OAUTH_API_URL=http://localhost:8080/realms/corp/protocol/openid-connect/userinfo

---

## 🔁 SSO Flow

1. User logs in via Keycloak
2. Redirect back to Grafana
3. Session established

---

## 🔐 MFA

- OTP required on login
- QR code enrollment via authenticator app

---

## 🧬 LDAP Federation

- LLDAP as identity source
- Users synced into Keycloak

---

## 🎟️ Token Example

{
  "sub": "user1",
  "realm_access": {
    "roles": ["user"]
  }
}

---

## 🛡️ RBAC Mapping

ROLE_ATTRIBUTE_PATH=contains(realm_access.roles[*], 'admin') && 'Admin' || contains(realm_access.roles[*], 'user') && 'Editor' || 'Viewer'

---

## ⚠️ Common Issues

Redirect URI must include:
http://localhost:3000/*

RBAC issues:
- Ensure roles exist in token
- Ensure mapping is correct

---
