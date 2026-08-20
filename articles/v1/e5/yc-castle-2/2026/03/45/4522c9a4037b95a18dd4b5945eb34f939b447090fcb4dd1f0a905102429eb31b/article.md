---
schema_version: "1.0.0"
document_id: "4522c9a4037b95a18dd4b5945eb34f939b447090fcb4dd1f0a905102429eb31b"
company_key: "yc-castle-2"
company: "Castle"
source_id: "yc-castle-2-news-import-b99476926256"
canonical_url: "https://docs.castle.io/changelog/ios-sdk-400-released"
published_at: "2026-03-19T00:00:00+00:00"
first_seen_at: "2026-07-21T12:42:16.969450+00:00"
fetched_at: "2026-07-28T21:26:23.229623+00:00"
content_hash: "sha256:f3ca77a25309b25771caaf2894fa1431b5491bbd9325b69be65cc72490b2885c"
---

# IOS SDK 4.0.0 Release

[Back to All](https://docs.castle.io/changelog)


Improved


Version 4.0.0 is a major update to the Castle iOS SDK. This release includes several improvements detailed below. Please review the breaking changes below before upgrading.


###


What's new


- **Expanded device signals** : Additional device signals collected for broader detection coverage.
- **SDK protection** : Added obfuscation to protect SDK internals from reverse engineering.
- **Swift-native** : Legacy Objective C parts of the SDK have been rewritten in Swift for improved stability and performance.
- **Async configuration** : New async variant of configure (configureAsync) that can be safely called from any thread or Swift Task.


##


Breaking changes


- ` import Castle` has changed to` import CastleSDK`
- ` Castle.configure()` now throws instead of asserting on invalid configuration
- New` Castle.configureAsync()` for async configuration call
- ` createRequestToken()` returns` nil` instead of an empty string when the SDK is not initialized
- Minimum supported iOS version is now iOS 13


##


Resources


- [GitHub Release](https://github.com/castle/castle-ios/releases/tag/4.0.0)
- [iOS SDK v4 Migration Guide](https://docs.castle.io/docs/ios-sdk-migrating-from-v3-to-v4)
- The release package includes Jazzy-generated API documentation with detailed technical reference for all public APIs.
