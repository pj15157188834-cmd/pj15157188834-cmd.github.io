# A Reproducible Android APK Evidence Model

When an Android APK is published outside Google Play, a download button alone does not tell us much about the file.

A useful APK record should make it possible for another person to independently confirm what application the file belongs to, which version it contains, whether the file has changed, and which signing identity is associated with it.

For that reason, I prefer an evidence model built from several technical fields rather than a filename or app title alone.

## 1. Package name

Every Android application has a package identifier, for example:

`com.example.app`

The package name is one of the most important identity fields because two APK files can display the same app name while belonging to different Android packages.
Tools such as `aapt` can expose this information.

Command: `aapt dump badging app.apk`

Typical output may contain:

`package: name='com.example.app' versionCode='120' versionName='1.2.0'`

This gives us the package name, version name and version code directly from the APK.

## 2. Version name and version code

These values should be recorded separately.

For example:

- Version Name: `1.2.0`
- Version Code: `120`

Two files can use the same visible version name while containing different internal version codes.

That means the visible version alone is not enough to identify a specific build.

## 3. APK file size

File size is not proof of authenticity, but it is useful supporting evidence.

For example:

- APK Size: `34.5 MB`

If another file claims to be the same version but is dramatically different in size, that difference deserves investigation.

## 4. SHA-256 of the APK

A SHA-256 hash identifies the exact file.

Command: `sha256sum app.apk`

If even one byte changes, the resulting SHA-256 value changes.

This makes it useful for:

- detecting file replacement;
- comparing two downloads;
- documenting the exact APK that was inspected;
- confirming that a stored file still matches an earlier record.

A hash proves file identity. It does not prove that an application is universally safe.

## 5. Signing certificate fingerprint

Android APKs are digitally signed.

Certificate information can be inspected with:

`apksigner verify --print-certs app.apk`

A useful field is:

`Signer certificate SHA-256`

This should be stored separately from the APK file hash.

The APK hash identifies the exact file.

The certificate fingerprint helps identify the signing identity associated with the APK.

A new application version will normally have a different APK SHA-256, while signing continuity may remain the same.

## 6. Minimum Android requirement

Compatibility information should also be recorded.

For example:

- Minimum Android: `Android 5.0+`

A file may be technically valid but still incompatible with an older device.

Compatibility therefore belongs in the technical record even though it is not itself proof of identity.

## 7. Source information

The APK source and the APK identity answer different questions.

Useful source fields include:

- source page;
- final download location;
- redirect path when relevant;
- date checked.

The source tells us where the APK was obtained.

The hash tells us exactly which file was obtained.

A download URL may remain unchanged while the file behind it changes, which is why both fields matter.

## 8. Check date

Every technical record should include a date.

For example:

- Checked: `24 September 2026`

Versions change, sources change and downloadable files can be replaced.

A check date makes it clear when the evidence was collected.

## 9. Keep technical evidence separate from commercial claims

APK evidence can establish facts such as:

- package identity;
- version information;
- file hash;
- signing certificate;
- Android compatibility.

It does not automatically establish:

- bonus amounts;
- payment methods;
- withdrawal support;
- login availability;
- promotional conditions.

Those claims require separate evidence.

A technically verified APK should not be used as proof of unrelated commercial information.

## 10. A compact reproducible record

A practical record can contain:

- App Name
- Package Name
- Version Name
- Version Code
- APK Size
- Minimum Android
- APK SHA-256
- Certificate SHA-256
- Source
- Check Date

Another person should be able to inspect the same APK and reproduce these fields.

That is what makes the record useful.

## Practical implementation

I use a similar evidence structure while maintaining [GameAPKEntry](https://www.gameapkentry.com/), where Android app records document package identity, version information, APK hashes, signing-certificate fingerprints and controlled download information.

The purpose is not to make absolute safety claims.

The purpose is to make the identity of the published APK easier to reproduce, compare and audit.

## Final checklist

Before publishing an APK record, verify:

- package name;
- version name;
- version code;
- APK size;
- minimum Android requirement;
- APK SHA-256;
- signing certificate SHA-256;
- source;
- check date.

A filename is only a label.

A reproducible technical record provides evidence.
