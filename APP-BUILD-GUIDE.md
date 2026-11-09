# Rely native apps

This repository now includes a Tauri 2 wrapper and GitHub Actions build pipelines.

## App behavior

The installed native shell opens https://rely.kz/ directly. It does not bundle a second copy of the web app or duplicate authentication. Rely account authentication remains website-owned; Google, GitHub and Discord sign-in do not require Apple login.

## Build locally

Requires Node 22, Rust stable, and the platform SDKs. Run the Tauri CLI with npx:

```sh
npx --yes @tauri-apps/cli@2 icon public/rely-icon.svg
npx --yes @tauri-apps/cli@2 build
```

## CI builds

The workflow .github/workflows/native-release.yml builds:
- Windows NSIS .exe and MSI
- Android debug-signed APK
- Linux AppImage and DEB
- macOS DMG

It uploads build artifacts and publishes a GitHub prerelease. The downloads page queries the public GitHub Releases API and only enables a download link when the corresponding binary asset exists.

## iOS

The iOS build is optional because installing an IPA directly on a physical iPhone requires Apple Ad Hoc signing and a provisioning profile containing that device's UDID. This does not make Apple login necessary for Rely accounts. Set the repository variable ENABLE_IOS_RELEASE=true and configure secrets IOS_CERTIFICATE_BASE64, IOS_CERTIFICATE_PASSWORD, IOS_PROVISIONING_PROFILE_BASE64, and APPLE_DEVELOPMENT_TEAM before enabling an iOS IPA build.

## Current limitations

- Windows installers are not code-signed; Windows SmartScreen may warn.
- Android APK is debug-signed for preview/testing, not a production Play Store release.
- macOS DMG is unsigned and may require local approval to open.
- iOS direct installation is not produced unless Ad Hoc signing is configured.
