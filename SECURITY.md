# Security Policy

## Reporting a Vulnerability

ShowMate takes security seriously. If you discover a security vulnerability related to the app, please report it responsibly.

### How to Report

**Do NOT open a public GitHub issue for security vulnerabilities.**

Instead, please send an email to the project maintainer with:

1. Description of the vulnerability
2. Steps to reproduce
3. Potential impact
4. Any suggested fixes (if applicable)

### Scope

This security policy applies to:
- The ShowMate Android application on Google Play
- The ShowMate web companion at showmate-1317e.web.app
- Any APIs or services associated with ShowMate

### What's NOT in scope
- This showcase repository (contains no executable code)
- Third-party services (Firebase, TMDB API, etc.)

## Security Measures in ShowMate

- **Data Encryption**: All user data is encrypted at rest using AES-256 via SQLCipher
- **Key Management**: Encryption keys are stored in the Android Keystore system
- **Biometric Auth**: Sensitive sections require biometric authentication
- **Screenshot Prevention**: `FLAG_SECURE` prevents screen capture in private areas
- **Firebase App Check**: Play Integrity attestation prevents API abuse
- **ProGuard/R8**: Code obfuscation in release builds
