# OAuth

## 1. What is OAuth?

- **OAuth** (Open Authorization) is an **authorization framework** that allows third-party applications to **access a user's resources** (like their data) **without exposing their username and password**.  
- Instead of sharing credentials, the user authorizes a third-party app to access their resources via access tokens.

### 🔹 Example: Login with Google
When you click on "Login with Google" on a new app, that app uses OAuth to access your Google profile without getting your password.

---

## 2. OAuth Flow

1. User clicks on **"Login with Google"**.  
2. Client redirects to the **Authorization Server** (Google).  
3. User logs in and grants permission.  
4. Authorization Server returns an **authorization code** to the client.  
5. Client exchanges this code for an **access token**.  
6. Client uses the access token to access resources from the **Resource Server**.

**Note:** The above flow is for **Authorization Code Grant Type**.

---

### 🔹 Tokens

- **Access Token:** Short-lived token used to access resources.  
- **Refresh Token:** Used to get a new access token when the old one expires, without re-authenticating.

---

## 3. Access Token vs Refresh Token

- When we hit the OAuth endpoint, the **Authorization Server** returns:
  - **Access Token**  
  - **Refresh Token**  
  - **Expiry times** for both tokens  

- **Access Token:**  
  Used in API calls to fetch data. It has a short lifetime.  

- **Refresh Token:**  
  Has a longer lifetime. When the access token expires, the app uses the refresh token to request a new access token **without asking the user to log in again**.  

- When the refresh token also expires (after several days), the user must log in again to get a new access token and refresh token.

---

## 4. Callback URL

When creating a custom OAuth application, we need to specify a **Callback URL**.  
It is the URL where the user will be redirected **after successful authentication** to the data source in OAuth.

---

## 5. Grant Types

A Grant type defines **how the client gets the access token** from the authorization server.

**Types:**
- **Authorization Code:** (Already covered in OAuth flow) Most common.
- **Client Credentials:** Machine-to-machine, no user. Used when apps need to access internal APIs.
- **Implicit:** Token returned directly in browser. ⚠️ Deprecated.
- **Password (Legacy):** Directly send username & password to app. ⚠️ Not recommended.
- **Refresh Token:** Used to get new access token after expiry.

**Client ID & Secret Requirement:**
| Grant Type              | Client ID | Client Secret |
|-----------------------|-----------|---------------|
| Authorization Code    | ✅ Yes    | ✅ Yes |
| Authorization Code + PKCE | ✅ Yes | ❌ No |
| Client Credentials    | ✅ Yes    | ✅ Yes |
| Password (Legacy)     | ✅ Yes    | ✅ Yes |
| Implicit              | ✅ Yes    | ❌ No |

---

## 6. OpenID Connect (OIDC)

**OIDC** is an **authentication layer** built on top of OAuth.

- **OAuth:** Focuses on **authorization** (Can this app access the user's data?).  
- **OIDC:** Focuses on **authentication** (Who is this user?).

In OIDC, a `/userinfo` endpoint is called to get user details (name, email, etc.).

---

