# Single Sign-On with Microsoft Entra ID (SAML 2.0)

SAML is used rather than OIDC because this server has no outbound internet access: with SAML the browser carries
the sign-in between Entra and Project Tracker, so the server never needs to contact Microsoft.

## Part A — Project Tracker administrator (5 minutes)

1. Sign in as an administrator and open **Administration → Authentication → SAML providers → + SAML provider**.
2. Display name: `Microsoft Entra ID`. Save.
3. The provider page now shows the **Service provider** values. Copy these three for the Entra admin:
   - **Service entity ID** (Identifier)
   - **Assertion consumer service URL** (Reply URL) — ends in `/callback`
   - **Sign-on URL**: `https://<your host>/login`

## Part B — Entra administrator (10 minutes)

1. Entra admin centre → **Enterprise applications → + New application → Create your own application**.
   Name: `Project Tracker`, choose *Integrate any other application you don't find in the gallery*.
2. **Single sign-on → SAML**. In *Basic SAML Configuration* enter the three values from Part A.
3. *Attributes & Claims* — keep the defaults and make sure these claims exist:
   | Claim | Source |
   |---|---|
   | `emailaddress` | `user.mail` |
   | `givenname` | `user.givenname` |
   | `surname` | `user.surname` |
   | Unique user identifier (Name ID) | `user.userprincipalname`, format *Email address* |
4. *SAML Certificates* → download **Federation Metadata XML**.
5. **Users and groups** → assign the people or groups who should sign in.

## Part C — Finish in Project Tracker

1. Back on the SAML provider page, choose **Metadata → upload / paste XML** and paste the Federation Metadata XML.
   (Use paste, not the metadata URL — the server cannot fetch URLs from the internet.)
2. Mapping: email → `emailaddress`, first name → `givenname`, last name → `surname`, login → Name ID.
3. Enable **Limit self-registration** as required and save.
4. Test in a private browser window: the sign-in page now shows **Microsoft Entra ID**.

## Notes

- Two-factor authentication stays enforced for local accounts; SSO users are protected by Entra's MFA/Conditional Access.
- Keep at least one local administrator account as a break-glass login.
- SCIM provisioning needs Entra to reach this server over HTTPS from the internet; it is not possible on `localhost`.
