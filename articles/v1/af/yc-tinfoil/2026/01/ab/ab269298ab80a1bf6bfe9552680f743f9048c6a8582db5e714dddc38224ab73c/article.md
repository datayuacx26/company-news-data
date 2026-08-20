---
schema_version: "1.0.0"
document_id: "ab269298ab80a1bf6bfe9552680f743f9048c6a8582db5e714dddc38224ab73c"
company_key: "yc-tinfoil"
company: "Tinfoil"
source_id: "yc-tinfoil-news-import-a84300979e46"
canonical_url: "https://tinfoil.sh/blog/2026-01-26-private-url-sharing"
published_at: "2026-01-26T12:00:00+00:00"
first_seen_at: "2026-07-22T16:45:23.137007+00:00"
fetched_at: "2026-07-28T22:23:07.215004+00:00"
content_hash: "sha256:fbe5a54d27c1910b64eea3774d1020714d58c0308060ff3d9407a8e5bcfb4044"
---

# Sharing Conversations Without Sharing With Tinfoil

[← Back to Posts](https://tinfoil.sh/blog)


# Sharing Conversations Without Sharing With Tinfoil


Jan 26, 2026


•


5 min read


Sacha Servan-Schreiber


Sometimes you want to share a conversation you had with an AI by sending a link to a colleague or a friend. Maybe you worked through a tricky debugging session, or the model produced a useful analysis you want to pass along.


With existing chatbots, implementing a sharing feature isn't challenging because the provider sees the whole conversation already. But with Tinfoil Chat, the conversation is encrypted with a personal key that is never exposed to anyone. A naive solution is to let users copy and send the full conversation via email, but this is cumbersome and falls short of the one-click link-sharing experience people expect.


## How non-private sharing works


With other (non-private) chatbots, the typical sharing flow looks something like this:


1. You click "share"
2. Your conversation gets copied to a database in cleartext
3. They generate a short link like` chat.com/share/abc123`
4. Anyone with the link can fetch the conversation from their servers


This approach does not work with Tinfoil because it would require you to share your personal encryption key with us or the person you're sharing the conversation with. This breaks the purpose of the unique encryption key[used to encrypt your chats](https://tinfoil.sh/blog/2025-09-24-private-chat-backups-local-first) .


## The URL Fragment Trick


A[URL fragment](https://en.wikipedia.org/wiki/URI_fragment) is the data in the URL delimited by` #` , typically used by websites to store information that should only be accessible by the client-side JavaScript. Importantly, browsers never send the fragment data to servers.


When you visit` example.com/page#some-data` , your browser sends a request for` /page` to the backend server but keeps` #some-data` to itself. This makes it possible to share a link encoding the full conversation data in the fragment. We could take the conversation, encode it into a URL-friendly base64 format, and generate a shareable link like so:


[https://chat.tinfoil.sh/share#data=](https://chat.tinfoil.sh/share#data=) <conversation>


**Example URL:**


[https://chat.tinfoil.sh/share#H4sIAAAAAAAAA41V227bRhD9lQH9UBtQ2cRtHVt5ctsEcFGgQew81QU0Wo7IhclddncpRQgC+BcK9KVf0X/yl/TMkpLl9PpiC8vZmTlnzpn9UKyL+fNZkWxqpZgX797+QK8D1524RFcV/tqVlVDMik5i5FpiMf/pQxF8jh5i/mS8S4jEwav3fcvWUfLUCTV+Q5pwNSWMtPHhjvDdO6GecRy4b8pC6yN/4q5HNy/Ozs9enJ6fnj0/ffZxti/GMVqEoM5hxStHrEVmlBrZVyK7b51szJ98n6x33GrdhBNOtPJt6zcRCRqODUVbOzpeHC1OiF1FxqMl4lVCDnZb+mWQsKWYgnX1S7KoEd3D/e+JohYEYq0CQtaIXw76mfrgDWiTirSbIO2WltscZ1qrt46lrMsZGtjIkpYBzeCyRz004+pWHu5/7UE6/nHfK7pOlMKTki6N8aFCkFZ++/rbh/s/vrw4P/uUhkiDlkdM7+3YJnL3YsCNodYbVlZoY1OjY8PdIEAoa1wKEv0QDKr/lgJXduRvxNBxMo1WZ0fSihYDF5EWtloogIVDqwvihGQgA8SM/OwwGtyLBpNtR+ZYaTRaAdVwH4eY8Vq2E1NgIGKmoJ+TzCgOpiGOFPyQtAvrVj50IxbguH5zGcekwCyhB6anvCB9EFehkcquVhL0bG0FWlAmkJTQd11LyBBpNbSqm1rZaT1XJX0jhsHs06xOdPZB2DQSD+QwU7Uo4iUG37CrQe7aMn3Pa742wfZpX5aRJGWXBIHgYoI4VKTaR+y8T82Mqi3IxfQcr209Yla9Lr2/6zjcaahfAfkknv2UY1YWdJRJjP9ou6/OzvEF7dRNgt2LGwBRp9OG1cM6cbW529P9xM3ESwXyxPclXQHXpMPgq8EIkGLAxoJDNQvKBS34r6lLeodwIAi+DxYYKIlpnDVwNcbcxfnfL4Ddz0qVm61+fHQyOVv77Flpdf7Ryru5PRp4ud17dtLwiQZOMxD9XXkz5NrsDOAo29l8O0FColOOz7OWJ/GW9Cb4tR7cQe6s5kxAAk/T6OhKjFenz/JCtZOo2aRRrbRksDKtQE5/WQD7xacigSFHdY7oNfZwr41CEnQqeecpWdg1bdR97vJQRq9iN+68jFz6PQ5977FZH1097YWxSVt9oSuhvziA2mOhL7FOnmTuPFyZCXtcd3mJjYPT2dhuaJXyA/3v7KOmVBgHXo3lrbt1Px7q6CVmDaFhqdH0kJR0jfutYqdXWLxWYV97uvpMjT8pVkGnvPRaeydzuv3/j89/vDlHJ2VZ3hawCfjbN7J7BA4NcJPZB+iI9PFxfhnkTTOoPeNGQjY3etVt8N0QMkvF/Fl58fXFx5/xfmJJJaku06fP7Z9NgtrnEAgAAA==](https://chat.tinfoil.sh/share#H4sIAAAAAAAAA41V227bRhD9lQH9UBtQ2cRtHVt5ctsEcFGgQew81QU0Wo7IhclddncpRQgC+BcK9KVf0X/yl/TMkpLl9PpiC8vZmTlnzpn9UKyL+fNZkWxqpZgX797+QK8D1524RFcV/tqVlVDMik5i5FpiMf/pQxF8jh5i/mS8S4jEwav3fcvWUfLUCTV+Q5pwNSWMtPHhjvDdO6GecRy4b8pC6yN/4q5HNy/Ozs9enJ6fnj0/ffZxti/GMVqEoM5hxStHrEVmlBrZVyK7b51szJ98n6x33GrdhBNOtPJt6zcRCRqODUVbOzpeHC1OiF1FxqMl4lVCDnZb+mWQsKWYgnX1S7KoEd3D/e+JohYEYq0CQtaIXw76mfrgDWiTirSbIO2WltscZ1qrt46lrMsZGtjIkpYBzeCyRz004+pWHu5/7UE6/nHfK7pOlMKTki6N8aFCkFZ++/rbh/s/vrw4P/uUhkiDlkdM7+3YJnL3YsCNodYbVlZoY1OjY8PdIEAoa1wKEv0QDKr/lgJXduRvxNBxMo1WZ0fSihYDF5EWtloogIVDqwvihGQgA8SM/OwwGtyLBpNtR+ZYaTRaAdVwH4eY8Vq2E1NgIGKmoJ+TzCgOpiGOFPyQtAvrVj50IxbguH5zGcekwCyhB6anvCB9EFehkcquVhL0bG0FWlAmkJTQd11LyBBpNbSqm1rZaT1XJX0jhsHs06xOdPZB2DQSD+QwU7Uo4iUG37CrQe7aMn3Pa742wfZpX5aRJGWXBIHgYoI4VKTaR+y8T82Mqi3IxfQcr209Yla9Lr2/6zjcaahfAfkknv2UY1YWdJRJjP9ou6/OzvEF7dRNgt2LGwBRp9OG1cM6cbW529P9xM3ESwXyxPclXQHXpMPgq8EIkGLAxoJDNQvKBS34r6lLeodwIAi+DxYYKIlpnDVwNcbcxfnfL4Ddz0qVm61+fHQyOVv77Flpdf7Ryru5PRp4ud17dtLwiQZOMxD9XXkz5NrsDOAo29l8O0FColOOz7OWJ/GW9Cb4tR7cQe6s5kxAAk/T6OhKjFenz/JCtZOo2aRRrbRksDKtQE5/WQD7xacigSFHdY7oNfZwr41CEnQqeecpWdg1bdR97vJQRq9iN+68jFz6PQ5977FZH1097YWxSVt9oSuhvziA2mOhL7FOnmTuPFyZCXtcd3mJjYPT2dhuaJXyA/3v7KOmVBgHXo3lrbt1Px7q6CVmDaFhqdH0kJR0jfutYqdXWLxWYV97uvpMjT8pVkGnvPRaeydzuv3/j89/vDlHJ2VZ3hawCfjbN7J7BA4NcJPZB+iI9PFxfhnkTTOoPeNGQjY3etVt8N0QMkvF/Fl58fXFx5/xfmJJJaku06fP7Z9NgtrnEAgAAA==)


Because the URL itself contains the entire conversation data, it can be "unpacked" by the receiving app of the person you're sharing the chat with. Note that this can be done without ever needing to provide your personal encryption key. Moreover, the personal chat is stored entirely in the fragment data, which means that only the browser sees the chat data when you paste this link. It's entirely decoded by the front-end application and no conversation data is sent over the internet when opening such a fragment-encoded URL. This ensures the conversation remains private from everyone but yourself and the person you share the URL with.


### What about really long conversations?


The URL fragment trick works for relatively short conversations, but what happens with longer conversations? It turns out that URL fragments can be used to encode a surprisingly large amount of data.[Chrome supports URLs that are up to 2MB](https://chromium.googlesource.com/chromium/src/+/HEAD/docs/security/url_display_guidelines/url_display_guidelines.md#url-length) in size and most other browsers support up to 1MB of data in the URL. Safari has virtually no limit on URL length.


But there are two problems: (1) conversations with images or documents can exceed 1MB and (2) the resulting links would be *really* long and cumbersome to share.


Additionally, the size limits of URLs are not just imposed by browser specs but could be imposed ad hoc by the medium via which you share the URL itself. Messaging tools like Slack truncate long URLs automatically, iMessage can mangle them, and other apps can cut off the URL after a few thousand characters.


## Full solution using a throwaway key


Our final design combines URL fragments with encryption. We were inspired by[Bitwarden Send](https://bitwarden.com/help/send-encryption/) ,[Mozilla Send](https://github.com/mozilla/send/blob/master/docs/encryption.md) , and[Excalidraw](https://plus.excalidraw.com/blog/end-to-end-encryption) , which use this technique for privacy-preserving file sharing. The idea is to first generate a throwaway encryption key that is independent of your personal Tinfoil key. The conversation is then (re)encrypted under this throwaway key and uploaded to Tinfoil. Because nobody but you has access to this throwaway key, neither Tinfoil nor anyone else can see the raw conversation data.


The shareable link contains the conversation ID in the path and the throwaway key in the fragment:


[https://chat.tinfoil.sh/share/](https://chat.tinfoil.sh/share/) <conversation-id>


# <secret-key>


Tinfoil servers only see that *something* was shared, and roughly how big it is, but have no way of reading the content. Only the person receiving the shared URL can recover the conversation.


When you delete a conversation you've shared, we delete the encrypted blob too. Anyone who clicks the link afterward cannot retrieve it anymore, even though they have the throwaway secret key.


## Conclusion


This sharing feature is a small example of a principle we keep coming back to at Tinfoil: designing a system where privacy is structural rather than promissory and achieving this without sacrificing user experience. We built our sharing system so that when we claim "Tinfoil can't read your shared conversations", anyone can verify this fact at the architecture level.


### Subscribe for Updates


[RSS Feed](https://tinfoil.sh/feed.xml)


Stay up to date with our latest blog posts and announcements.


[Previous Post](https://tinfoil.sh/blog/2026-01-22-private-ai-web-search)[Next Post](https://tinfoil.sh/blog/2026-02-03-proving-model-identity)
