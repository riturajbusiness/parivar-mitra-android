# Parivar Mitra Android Customer App V1 — API Contract

Base: `https://parivarmitra.com/api/v2`

## Authentication

- `GET /bootstrap` — public app configuration
- `POST /auth/request-otp` — request email-only OTP
- `POST /auth/verify-otp` — verify six-digit email code
- `POST /auth/complete-profile` — complete a brand-new Customer account after verified email
- `POST /auth/refresh` — rotate access + refresh token
- `POST /auth/logout` — revoke current mobile session

Authenticated requests use:

`Authorization: Bearer <access_token>`

Mobile access tokens expire after 30 minutes. Refresh tokens rotate and expire after 30 days. Tokens are stored hashed server-side; plaintext tokens are returned only to the app.

## Customer

- `GET/PATCH /me`
- `GET /home`
- `GET /services`
- `GET /services/{id}`
- `GET /family`
- `POST /family/beneficiaries`
- `POST /family/properties`
- `GET/POST /requests`
- `GET /requests/{id}`
- `GET /requests/{id}/repeat`
- `GET/POST /notifications`
- `GET /payments`
- `GET /family-summary`
- `POST /ai/chat`

## V1 booking rule

Android V1 creates the Customer request inside the existing Admin operations workflow and returns a server-side estimate. It deliberately does not duplicate the website's Razorpay checkout logic. Final scope/payment instructions continue through the existing controlled workflow until the dedicated native payment release is implemented.

## Security boundaries

- Customer mobile tokens cannot authenticate Admin/Super Admin/Partner/Coordinator roles.
- The public OTP request endpoint does not reveal whether a privileged email exists.
- New accounts are created only after a valid, recent OTP challenge.
- Mobile API access is independent of browser sessions and CSRF cookies.
- All request/family/payment reads are scoped by the authenticated Customer ID.

## Database migration

`ParivarMitra_V5.19_ANDROID_APP_V1_UPGRADE.sql` is required before mobile authentication. It adds only app-session/push-token tables and does not change existing SEO/customer/service/payment tables.

## App headers

The Flutter app sends:

- `Authorization: Bearer <access token>` on authenticated calls
- `X-App-Version`
- `X-Platform: android`
- `X-Device-ID`
- `X-App-Language: en|hi|mr`

No database credentials are present in the Android source.
