---
schema_version: "1.0.0"
document_id: "124f7e33f03c92992d57a6f5691bebb968d4a47dfc8a931589171404e9f3f71b"
company_key: "yc-buildbuddy"
company: "BuildBuddy"
source_id: "yc-buildbuddy-rss-9eed3a34e5de"
canonical_url: "https://www.buildbuddy.io/blog/how-to-migrate-an-ios-app-to-bazel"
published_at: "2024-03-13T08:00:00+00:00"
first_seen_at: "2026-07-25T01:07:54.250593+00:00"
fetched_at: "2026-07-28T22:26:08.375343+00:00"
content_hash: "sha256:e7e4f4724a62aee5c88de6a0982919074c591c2a6af810f21a95b5438efc5a64"
---

# How to migrate an iOS app to Bazel

**rules_apple** processes` Info.plist` and` .entitlements` files in a slightly different manner than Xcode. In Xcode, you have build settings such as` PRODUCT_BUNDLE_IDENTIFIER` ,` CURRENT_PROJECT_VERSION` , and` PRODUCT_MODULE_NAME` . And when it processes an` Info.plist` , it substitutes references to those build settings with their resolved value. **rules_apple** doesn't use build settings, so it's not able to do the same substitutions.


There are some attributes on bundling rules that **rules_apple**[will substitute](https://github.com/bazelbuild/rules_apple/blob/3.3.0/doc/common_info.md#variable-substitution) in place of a build setting, such as` bundle_id` for` PRODUCT_BUNDLE_IDENTIFIER` , but for the most part when using Bazel you'll need to remove these build setting references from` Info.plist` and` .entitlements` files, or use a custom rule to expand those values for you. I decided to use the **bazel_skylib**[expand_template](https://github.com/bazelbuild/bazel-skylib/blob/1.5.0/docs/expand_template_doc.md) rule to allow the build setting references to stay in the` Info.plist` files.
