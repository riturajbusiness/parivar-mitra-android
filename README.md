# Parivar Mitra Customer Android App V1

Flutter customer application for Parivar Mitra. This app is **not a WebView**. It uses the Parivar Mitra `/api/v2` bearer-token API and the same production customer/service/request data as the website.

## V1 capabilities

- Email-only OTP sign-in (no mobile-number login)
- New-customer email verification + profile completion
- Secure access token + rotating refresh token
- English / Hindi / Marathi UI foundation
- Customer Home / Family Command Center
- AI Family Assistant + speech input
- Services catalogue
- Customer request creation
- My Services + request timeline
- Family 360: people + properties
- Notifications
- Invoices/payment history
- Monthly NRI/Family Summary
- Profile/language/security session logout

## Backend requirement

Deploy the website/API patch and import `ParivarMitra_V5.19_ANDROID_APP_V1_UPGRADE.sql` before attempting app login.

## Build prerequisites

1. Install a current stable Flutter SDK and Android Studio.
2. Run `flutter doctor`.
3. In this directory run `flutter pub get`.
4. Create `android/local.properties` automatically by running Flutter once or set `flutter.sdk=/path/to/flutter`.
5. Run `flutter analyze`.
6. Run `flutter test`.
7. Run `flutter build apk --release` for APK or `flutter build appbundle --release` for Play Store AAB.

## Signing

The included Gradle file uses debug signing as a safe development placeholder. Before Play Store release create a private upload keystore and configure release signing using Android/Flutter standard signing configuration. Never commit the keystore or passwords.

## API base URL

`lib/core/config.dart` points to `https://parivarmitra.com/api/v2`. Use a staging URL while testing server migrations and authentication.

## Important V1 behavior

Mobile request submission enters the existing Admin review workflow. The API returns the server-side estimate, but final scope/payment instructions remain human-reviewed in V1. Direct Razorpay Android SDK checkout is intentionally deferred to the next mobile release rather than duplicating payment logic unsafely.

## Source bundle note

The source bundle intentionally does not vendor the Gradle wrapper JAR binary. The included GitHub Actions workflow seeds the standard wrapper from the installed Flutter SDK before analysis/build. On a developer machine, running `flutter create --platforms=android .` with a current Flutter SDK can also restore standard generated wrapper files if needed; preserve the Parivar Mitra package/application settings after regeneration.

Customer-facing login remains **email-only**. Phone/mobile login is not part of Android Customer App V1.
