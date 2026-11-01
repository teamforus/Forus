# Login through an existing browser session

## Use Case

To login a user into the app with a PIN code, using a browser in which the user is already logged in, for example on a computer. This is also how the Mobile login flow continues when the user opens the login email on a computer instead of on their phone.

## Flow

Your app creates a pending login and shows a 6-digit PIN code. The user types that code into a browser where they are already logged in, and the browser approves the pending login. Only the calls in the "Mobile app" box are made by your app; the browser calls are made by the Forus web frontends.

- Keep the `access_token` from the first call secret and only show the `auth_code`. The `access_token` becomes usable once the code is approved.
- The PIN code is valid for 10 minutes. `check-token` keeps returning `pending` after that, so stop polling after 10 minutes and request a new code.
- If the browser session has completed 2FA, the app session inherits that. If the user needs 2FA but the browser session has not completed it, protected calls from the app return `401 {"error":"2fa"}`. The user then has to complete 2FA in the browser first and repeat this flow.

``` mermaid
graph TD
  subgraph App [Mobile app]
    A["POST /api/mobile/requester/v1/identity/proxy/code returns access_token (pending) and auth_code"]
    B["show the 6-digit auth_code to the user"]
    C["GET /api/mobile/requester/v1/identity/proxy/check-token with header Access-Token: access_token"]
    D{"message?"}
    E["wait a few seconds"]
    F{"10 minutes passed?"}
    I["User is logged in"]
  end

  subgraph Browser [In Browser, user already logged in]
    W1["user opens the PIN form: Autoriseer apparaat in the user menu of the webshop or dashboard, or the email landing page on a computer"]
    W2["user types the auth_code"]
    W3["POST /api/v1/identity/proxy/authorize/code with auth_code, Bearer: browser session"]
    W4["pending login is now active, 2FA state copied if the browser session is 2FA confirmed"]
  end

  A --> B --> C --> D
  D -->|pending| F
  F -->|no| E --> C
  F -->|yes: request a new code| A
  D -->|invalid: request a new code| A
  D -->|"active: set access token in device's secure storage"| I
  B -.->|user reads the code| W1
  W1 --> W2 --> W3 --> W4
  W4 -.->|next check-token returns active| D
```
