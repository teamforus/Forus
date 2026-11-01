# Login through an existing provider mobile app session

## Use Case

To login a user into the provider app on a new device with a QR code, using another device on which the provider app is already logged in.

## Flow

The provider app on the new device creates a pending login and shows a QR code. The provider app that is already logged in scans that QR code and approves the pending login. Your app implements both sides: showing the QR code on the login screen, and scanning QR codes once logged in.

- Keep the `access_token` from the first call secret and only put the `auth_token` in the QR code. The `access_token` becomes usable once the QR code is approved.
- The Forus provider dashboard encodes its QR code as `{"type":"auth_token","value":"<auth_token>"}`. Use the same format, so one scanner handles both. A logged-in provider app can approve the QR code on the provider dashboard sign-in page with the same call, which logs the user into the dashboard.
- The QR code is valid for 60 minutes. `check-token` keeps returning `pending` after that, so stop polling after 60 minutes and request a new QR code.
- The approve call returns `{"success": true}`. It returns `404` for an unknown `auth_token`, and `403` when the QR code was already used or has expired.
- If the scanning session has completed 2FA, the new session inherits that. If the user needs 2FA but the scanning session has not completed it, protected calls from the new device return `401 {"error":"2fa"}`.

``` mermaid
graph TD
  subgraph New [Provider app on the new device, not logged in]
    A["POST /api/mobile/provider/v1/identity/proxy/token returns access_token (pending) and auth_token"]
    B["show a QR code containing the auth_token"]
    C["GET /api/mobile/provider/v1/identity/proxy/check-token with header Access-Token: access_token"]
    D{"message?"}
    E["wait a few seconds"]
    F{"60 minutes passed?"}
    I["User is logged in"]
  end

  subgraph Existing [Provider app, already logged in]
    S1["employee scans the QR code"]
    S2["POST /api/mobile/provider/v1/identity/proxy/authorize/token with auth_token, Bearer: own session"]
    S3["pending login is now active, 2FA state copied if this session is 2FA confirmed"]
  end

  A --> B --> C --> D
  D -->|pending| F
  F -->|no| E --> C
  F -->|yes: request a new QR code| A
  D -->|invalid: request a new QR code| A
  D -->|"active: set access token in device's secure storage"| I
  B -.->|employee scans the code| S1
  S1 --> S2 --> S3
  S3 -.->|next check-token returns active| D
```
