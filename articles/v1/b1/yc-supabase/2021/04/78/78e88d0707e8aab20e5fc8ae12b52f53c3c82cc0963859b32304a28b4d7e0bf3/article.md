---
schema_version: "1.0.0"
document_id: "78e88d0707e8aab20e5fc8ae12b52f53c3c82cc0963859b32304a28b4d7e0bf3"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/supabase-nft-marketplace"
published_at: "2021-04-01T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:ea5be0bb5419fc11bd50ab1226f645003fbe0f57566f13e592fedb38a90aaf57"
---

# Supabase Launches NFT Marketplace

### Crypto at Supabase#


Most people don’t know this but the Supabase team has a long history of involvement in the NFT game. In 2018,[Copple](https://www.blockpunk.net/en/collection/paul-copplestone) and[Rory](https://www.blockpunk.net/en/collection/rory-wilding) were minted as NFTs on[Blockpunk.net](https://www.blockpunk.net/) , an Ethereum based Anime-NFT platform that four Supabase core team members helped build. We also helped in the building of the world’s first tokenised anime premiere -[Vevara in Your Dream](https://www.animationmagazine.net/anime/first-tokenized-anime-film-vevara-in-your-dream-debuts/) .


The team were also founding members of the[Guacchain Foundation](https://github.com/awalias/guacchain) , an economic experiment in a deflationary-based monetary system, inspired by the fact that Avacados ripen faster when stored together.


NFTs are now capturing the imagination of speculators everywhere, but NFTs as they exist today have a major flaw. We call it the *Copy-Paste Problem* . The Copy-Paste problem is when a token contains a URL to an image hosted on the web, or even on IPFS; there is nothing stopping someone who does not own the token, from heading to that URL, and just downloading the image for themselves.


As our latest foray into NFTs we’re announcing[BuyMeth.com](https://buymeth.com/) , (Meth = \[M\]isleading \[E\]ncrypted \[Th\]umbnails) an NFT marketplace that solves the copy-paste problem of NFTs today.


### How does it work?#


Each NFT stores a blurhash representation of a full image, but also a link to the full encrypted version of the image stored on IPFS. The full image is always encrypted with the key of the *current* owner.


When a sale is initiated i.e. the seller accepts a buyers offer, Metamask re-encrypts the image with the public key of the buyer, and uploads it to IPFS, the url in the NFT is updated and the buyer assigned as the new owner. After the sale, a 1 week challenge period is initiated where the proceeds of the sale are kept in an escrow contract. The buyer can then decrypt the contents of the image, and can run an automated verification function to verify that the image received (when hashed) does indeed produce the publically available blurhash (or thumbnail). If it does not, then the buyer can submit a claim to the sales contract, along with a proof of the invalid file, and claim back the proceeds.


As a way to dis-incentivise previous owners of the image to not reveal the original image, we bake in a royalty mechanism, whereby previous owners receive a fraction of the sale price each time, the artwork changes hands.


Despite the undeniable value of such a platform, this announcement is of course an April Fools joke. We are not launching a Supabase NFT platform; but we are[Launching a TON of other stuff this week](https://supabase.com/blog/launch-week) .
