# Mobile login flow

## Use Case

To login an existing or new user into the app. For a new user, an account is immediately created.

## Flow

2FA is checked and/or configured in the browser before the user is redirected to your app on his phone with a deep link. This part requires no work from you for your mobile wallet app implementation, but is included for clarity.

``` mermaid
graph TD
  A[mobile app sends user supplied email with POST /api/mobile/requester/v1/identity]
  B{email is known or unknown}

  C[user receives signin email with magic link]
  D[user receives signup email with magic link]
  subgraph Browser [In Browser]
    E["GET /api/mobile/requester/v1/identity/proxy/email/redirect/{emailToken}"]
    F["GET /api/mobile/requester/v1/identity/proxy/confirmation/redirect/{exchangeToken}"]
    P1["web page exchanges the email token for a browser session with GET /api/v1/identity/proxy/email/exchange/{emailToken}"]
    P2["web page exchanges the email token for a browser session with GET /api/v1/identity/proxy/confirmation/exchange/{exchangeToken}"]

    subgraph TwoFA [2FA]
      T1["GET /api/v1/identity/2fa"]
      T2{"required and not confirmed?"}
      T3{"user has an active 2FA method?"}

      S1["user picks a method"]
      S2["POST /api/v1/identity/2fa with type and phone (SMS only)"]
      S3["user receives SMS code, or scans secret_url QR with authenticator app"]
      S4["POST /api/v1/identity/2fa/{uuid}/activate with code and key"]

      L1["user picks one of the active methods"]
      L2["POST /api/v1/identity/2fa/{uuid}/resend (SMS only)"]
      L3["user enters SMS or authenticator code"]
      L4["POST /api/v1/identity/2fa/{uuid}/authenticate with code"]

      R1["web page reloads"]
      R2["GET /api/v1/identity/proxy/check-token confirms the browser session is still active"]
    end

    K{"web page opened on a phone?"}
    M1["POST /api/v1/identity/proxy/shared-2fa: new token, browser session logged out"]
    M2["web page shows PIN form, continue with Login through an existing browser session"]
  end

  H["Exchange new token for access token with GET /api/mobile/requester/v1/identity/proxy/email/exchange/{emailToken}"]
  I["User is logged in"]


  A --> B
  B -->|known| C
  B -->|unknown| D
  C -->|clicks link| E
  D -->|clicks link| F
  E -->|opens web page| P1
  F -->|opens web page| P2
  P1 --> T1
  P2 --> T1
  T1 --> T2
  T2 -->|no: not required, or already confirmed| K
  T2 -->|yes| T3
  T3 -->|no: setup| S1
  S1 --> S2 --> S3 --> S4
  T3 -->|yes: login| L1
  L1 -->|SMS| L2 --> L3
  L1 -->|authenticator| L3
  L3 --> L4
  S4 -->|browser session is now 2FA confirmed| R1
  L4 -->|browser session is now 2FA confirmed| R1
  R1 --> R2 --> T1
  K -->|yes| M1
  K -->|no: desktop| M2
  M1 -->|"opens mobile app with deep link: meapp-requester://identity-restore?token={new token}"| H
  H -->|set access token in device's secure storage| I
```