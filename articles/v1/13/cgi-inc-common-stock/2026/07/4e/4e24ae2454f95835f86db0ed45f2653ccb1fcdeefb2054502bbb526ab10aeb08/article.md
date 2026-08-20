---
schema_version: "1.0.0"
document_id: "4e24ae2454f95835f86db0ed45f2653ccb1fcdeefb2054502bbb526ab10aeb08"
company_key: "cgi-inc-common-stock"
company: "CGI Inc."
source_id: "cgi-inc-common-stock-rss-66ef697d2497"
canonical_url: "https://www.cgi.com/en/blog/cybersecurity/improving-passwords-to-protect-systems-while-preserving-user-convenience"
published_at: null
first_seen_at: "2026-07-20T23:21:24.029549+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:1c374a8465d39f85cc8f0a1a5a7acfee369e2e45bd4a9002502349a355b07e0e"
---

# Improving passwords to protect systems, while preserving user convenience

Passwords remain the single most common user authentication technology, but they are also the least secure. Here are a few things you can do to make your organization less vulnerable if you decide to continue using password-based authentication.


Password approaches all work the same way: your password is stored in a database on the computer somewhere, and when you type your password, the two values are compared. For the past 35 years or so, passwords have been “hashed,” which is a non-reversible cryptographic operation that converts your password into binary gibberish. When you enter your password initially, the computer hashes it and stores the hash value. Then when you log in to the computer, the computer hashes the password you give, and compares it to the hashed value stored in the database. If the two hashed values are identical, the two passwords are identical.


So why are passwords not so secure? Simply put, blame the gamers. The popularity and profitability of three-dimensional games has led to a two-decade long sustained effort to develop graphics cards capable of extremely high resolution and visual fidelity. The era of cinema-quality graphics displayed at high frame rates is almost upon us. At the heart of every graphics cards is a “GPU” (graphics processing unit) chip that is effectively a massively parallel computer with up to 3,000 tiny CPUs on it. These CPUs are only good at arithmetic, but that’s exactly what you need to compute hash values. Password hackers have learned to use $500 GPUs to compute hash values at blazing speeds, which allows them to try *billions* or even *hundreds of billions* of password guesses per second. This means that if attackers manage to get access to your database of hashed passwords, they can try a “brute force” attack to recover the plaintext passwords simply by trying every combination of letters/numbers/etc., until they find one that matches.


There is a two-part solution that can help protect you from these hackers. One is to strengthen your password hashing strategy. The other is to increase the length and complexity of the passwords themselves.


**Strengthening hashes**
We need to make the hashes so hard to compute that even billions per second is not enough to crack them. To strengthen your hashes, you must do three things.


1. **Increase the length of the hash itself** . For your hashing algorithm, use SHA-256 or (better) SHA-512 or (best of all) an algorithm that is specifically designed for password hashing such as SHA512crypt or bcrypt.
2. **Put “salt” on every user’s password** . “Salt” is randomly-generated text added to user passwords. Salting ensures that when two users pick the same password, the resulting hashes are different.
3. **Run multiple rounds of the hashing algorithm** . This is when you hash the password, hash the hash, hash that, etc. The idea is to make the password hashing process very slow so that an attacker has to work harder and longer to compute the final hash value.


**Lengthening passwords**
Longer, more complex passwords increase the number of tries an attacker has to make before finding a match. The problem with super-complex passwords is that users hate them. The solution is to use a keyring application on the user’s PC. This has another benefit: it requires the attacker to defeat the keyring on each PC instead of just executing one attack and stealing 10,000 users passwords from your server in one shot. If you have 10,000 users, the attacker would have to successfully crack 10,000 of your users’ phones or PCs 3—4 a much more difficult task than compromising one server.


Today, strong authentication is an increasingly important issue for enterprises when it comes to mobile users. While it is likely impossible to have perfect authentication security, taking care while engineering an authentication scheme can result in lowering risk to an acceptable level. Read more about this topic in my previous blog post on[strong mobile authentication](http://www.cgi.com/en/blog/cybersecurity/best-practices-for-strong-mobile-biometric-authentication) .
