🔵🟢🔴➡️⭕🟠🟦🟣🟥🟧✔️⏺️ ☑️ • ‣ → ⁕

# ⏺️ OAuth Terminology

## ➡️ Resource owner

It is the end user. In other words, end user owns the resources (email profile), that's why we call him as Resource owner

## ➡️ Client

The website, mobile app or api that is the one which interacts with Auth Server or Resource Server.

## ➡️ Authorization Server

- Handles authentication (who you are) and authorization (what you are allowed to do).
- This is the server where authorization logic acts as Authorization server; end user should have account in this server.
- Verifies the identity of the Resource Owner (user).
- Issues tokens (access tokens, refresh tokens) to the Client.
- Like **Security guard** → Checks your ID and gives you a visitor badge.

## ➡️ Resource Server

- Hosts and protects the actual resources (data) the client wants.
- Validates the access token issued by the Authorization Server.
- Returns the requested data if the token and scope are valid.
- Like **Office room** → When you enter, the guard at the door checks your badge and decides whether you’re allowed in.

## ➡️ Scopes

These are the granular permissions that the client wants, such as access to data or to perform certain actions. The auth server can issue an access token to client with the scopes of **Email**, **READ** etc.

# ⏺️ Need the answer of this question

- What info client share with the authentication server to get the ACCESS TOKEN?
  Is it the username password or what?
- And Which one is usually used today( modern spring boot or spring cloud microservices)

## ➡️ Answer

### 🟦 Modern Standard: Authorization Code Flow with PKCE

#### 🔵 Why?

- Secure (prevents stolen tokens/code interception).
- Works well for React/Angular/Vue frontends + Spring Boot backends.
- Username/password are never shared with the client.

#### 🔵 Flow:

- User is redirected to the Authorization Server login page (e.g., Keycloak, Okta, Auth0, or Spring Authorization Server).
- User logs in there (username/password).
- Auth Server sends an Authorization Code back to the client (React app).
- Client exchanges that code with the Auth Server → gets Access Token (JWT).
- Client uses Access Token to call Resource Servers (Spring Boot microservices).

### 🟦 Microservices → JWT (OAuth2 Access Tokens) + API Gateway

- **In Spring Cloud microservices world:**
  - Authorization Server issues a JWT access token.
  - The API Gateway (Spring Cloud Gateway) validates the token.
  - Downstream microservices trust the gateway or validate the JWT directly.
