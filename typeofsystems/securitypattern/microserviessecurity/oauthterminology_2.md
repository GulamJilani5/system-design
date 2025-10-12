🔵🟢🔴➡️⭕🟠🟦🟣🟥🟧✔️⏺️ ☑️ • ‣ → ⁕

# ⏺️ Oauth, OpenID and Keycloak

## ➡️ OAuth2 alone = Authorization framework

- **OAuth2** was designed to let applications access resources on behalf of a user.
- **Example:** **GitHub OAuth** allows **StackOverflow** to read your **GitHub profile** (after you approve).
- **OAuth2** tells you what the client can do, but it doesn’t tell you who the user is.
- It issues access tokens, not identity information.

## ➡️ OpenID Connect (OIDC): “Who the user is” (Identity Layer on top of OAuth2)

- **OAuth2 + Identity Layer**
- **OIDC extends OAuth2 by adding:**
  - ID Token (**JWT**) → contains user’s identity info (e.g., `name`, `email`, `sub/subject ID`).
  - A `/userinfo` endpoint → to fetch more user profile data.
  - Standardized scopes like openid, profile, email
- **With OIDC:**
  - The **Authorization Server** becomes an **Identity Provider (IdP)**.
  - After login, the client not only gets access token but also an **ID token** to know who the user is (authenticated identity).

### Example:

- **When you log in with Google on a website:**
  - OAuth2 part → gives the site permission to access your Google Drive or contacts.
  - OIDC part → gives the site information about you (your Google account identity).

### Why OIDC matters in Spring Boot:

- In a **React frontend** + **Spring Boot backend:**
  - The frontend gets ID Token to know the user’s identity (e.g., display “Hello Gulam”).
  - The backend uses Access Token to authorize API calls.
- It avoids building your own login/identity system.

## ➡️ Keycloak — Open Source Identity & Access Management Server

- **Keycloak** is an **Authorization Server + Identity Provider** that supports both:
  - **OAuth2** (authorization, token issuing).
  - **OpenID** Connect (identity layer).
  - **SAML** (legacy enterprise protocol).
- **What Keycloak does:**
  - Handles user authentication (login forms, password storage, MFA, etc.).
  - Issues **Access Tokens** and **ID Tokens (OIDC)** to clients.
  - Acts as central **Auth server** for all your Spring Boot microservices.
  - Manages users, roles, groups, permissions.
  - Can integrate with **LDAP**, **Active Directory**, or social logins (**Google**, **GitHub**, etc.).

## ➡️ Typical Spring Boot + Keycloak setup:

- **Frontend (React)** → redirects user to **Keycloak** login page.
- User logs in → **Keycloak** issues `access_token` + `id_token`.
- **Frontend** sends `access_token` to **Spring Boot API Gateway / microservices**.
- Each microservice validates the **JWT** `access token` (no DB needed).
- If needed, microservices can also use **Keycloak’s** `/userinfo` endpoint to get more user info.
