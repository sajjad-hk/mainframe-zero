---
tags: [tech, xpansiv]
topic: auth
---

# Xpansiv SSO — Local Dev

How to get the `sub` claim for `ema-localenv-ansible` configuration.

## Steps
1. Go to [SSO landing](https://dev.preprod.xpansivconnect.com/app/ssolanding) and log in
2. Call the endpoint:
   ```
   GET https://dev.preprod.xpansivconnect.com/app/api/v1/user/management/action/get-sso-logins
   Authorization: Bearer <your_token>
   ```
3. Decode the JWT at [jwt.io](https://www.jwt.io/) and extract the `sub` field
4. Use the `sub` value in your local env ansible config

## Notes
- Token is short-lived — get a fresh one each session
