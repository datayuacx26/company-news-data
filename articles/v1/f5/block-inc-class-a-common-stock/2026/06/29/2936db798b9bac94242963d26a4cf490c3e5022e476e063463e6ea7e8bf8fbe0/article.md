---
schema_version: "1.0.0"
document_id: "2936db798b9bac94242963d26a4cf490c3e5022e476e063463e6ea7e8bf8fbe0"
company_key: "block-inc-class-a-common-stock"
company: "Block Inc."
source_id: "block-inc-class-a-common-stock-rss-613ed2351e85"
canonical_url: "http://engineering.block.xyz/blog/encrypting-wireproto-messages"
published_at: "2026-06-02T12:00:00+00:00"
first_seen_at: "2026-07-20T03:32:06.876214+00:00"
fetched_at: "2026-07-28T20:49:52.299135+00:00"
content_hash: "sha256:d5ac4000a2b17e6da8e58bc1b148bcbdbd1765396adc83410cb28365c467e493"
---

# Encrypting Wire/Proto Messages

## Encrypting Wire Messages


In a[previous blog post](https://code.cash.app/encryption-using-data-keys) (a year and a half ago) we shared a small glimpse of a screenshot showing the amount of data we’re encrypting/decrypting on a daily basis. Want to know how we got to 9 (now 18) Terabytes daily? Simple. We send a ton of messages between our backend services each day, and like most modern microservice setups these days, we use Kafka topics to shoot these messages.


While securing communication channels and infrastructure-level controls are important, event-driven systems can benefit from additional protections at the message layer.


Leveraging our approach of application layer encryption we decided that besides securing the Kafka event bus infrastructure, we should also apply encryption to the messages themselves.


Almost all of our service-to-service communication is encoded as Protocol Buffer messages. And so, we decided to focus on supplying engineers with an easy to use encryption library specifically for proto messages.


### How


The main design feature of our encryption library relies on the ability to associate a message definition with a key name. We used a[custom proto option](https://protobuf.dev/programming-guides/proto2/#customoptions) extension to define a message option where the key name is specified:


```text
protobuf    1  /**
2   * Associate a key name with a message that is used to encrypt it.
3   *
4   * Messages that use the key_alias option must also define a bytes field named "ciphertext_", either inline or via an extension.
5   */
6   extend   google  .  protobuf  .  MessageOptions   {
7        optional     string   key_alias   =     50601  ;
8   }
```


We also expect any message that has the` key_alias` option to include a field called` ciphertext_` of type` bytes` . Our encryption library will use the` ciphertext_` field to store the encrypted version of the message fields. Here’s a simple hypothetical scenario of what this looks like in practice:


```text
protobuf    1  import     "common/crypto.proto"  ;
2   import     "common/enums.protos"  ;
3
4   message     Address     {
5        option     (  key_alias  )     =     "us_address_aead_key"  ;
6
7        string   line_1   =     1  ;
8        string   line_2   =     2  ;
9        State   us_state   =     3  ;
10        uint32   zip   =     4  ;
11        Country   country   =     5  ;
12
13        bytes   ciphertext_   =     10  ;
14   }
```


Lastly, in order to encrypt or decrypt this proto message, we expose 2 simple functions:


```text
kotlin    1  fun     <  M   :   com  .  google  .  protobuf  .  Message  <  *  ,     *  >  >     encrypt  (  message  :   M  )  :   M
2   fun     <  M   :   com  .  google  .  protobuf  .  Message  <  *  ,     *  >  >     decrypt  (  message  :   M  )  :   M
```


This is all that’s required!


The` encrypt` and` decrypt` functions are initialized in a class that’s baked into our common service framework, and will load all the encryption keys a service has configured on startup so there’s no additional setup required by engineers who use it.


But we’re not done yet. Having the ability to encrypt/decrypt simple messages like the one defined above is nice, but in real life things get much more complicated. A few common examples are, how can we handle nested messages? And how should we handle cases where a service is unable to decrypt a message because it doesn’t have access to its key?


We decided to solve these 2 specific questions by applying the following behavior to our message encryption library:


- Encryption is done **recursively, depth first** .
- Encryption will fail with an error thrown if a key cannot be used or is not accessible.
- Messages that are already encrypted are skipped.
- When decrypting a message, one can either:


- Specify a list of keys to ignore, in which case decryption will succeed even though not all parts of the message were decrypted.
- Call another function called` decryptPartially` which will return the (potentially partially) decrypted message + the list of key names that were not accessible to use.


- If a key could not be used during decryption, and wasn’t explicitly specified to be ignored (and not using the` decryptPartially` function), then decryption will fail with a thrown error.


With the above behavior, we’ve covered most of the message encryption and decryption needs we have at Block. For most services, this boils down to adding a single statement before/after transmitting messages in a Kafka bus or gRPC.


We’ll leave the implementation details and specifics as an exercise for the reader (or an agent?!)


### Why? What have we got out of this?


Besides the obvious advantage of securing our event data using application layer encryption, the above design uncovered a bunch of really nice features that we can leverage now.


First of all, direct and easy to discover association between keys and data definitions. The name of the key that’s responsible for securing a proto message is defined in the message definition itself, so it’s extremely easy to figure out. It also makes it easily searchable in GitHub.


The recursive behavior of the message encryption library now allows us to transmit more complex messages without needing to decompose and reconstruct these messages. Going back to the example used above, we can now send more complex data structures like:


```text
protobuf    1  message     CashUser     {
2        string   cash_tag   =     1  ;
3        string   bio   =     2  ;
4        Address   mailing_address   =     3  ;      // encrypted using "us_address_aead_key"
5        BankAccount   linked_bank_account   =     4  ;      // encrypted using "cashapp_banking_key"
6   }
7
8   message     Transaction     {
9        CashUser   sender   =     1  ;
10        CashUser   receiver   =     2  ;
11        uint32   amount   =     3  ;
12   }
```


In this scenario, different services receiving the` Transaction` message could only access the parts of the message they need access to. For example, the web UI service, which shouldn’t be able to read customers’ mailing addresses and definitely shouldn’t be able to access bank account information can still display the basic transaction details. Our banking service though, can receive the transaction and easily process it and credit/charge the relevant bank accounts. This behavior naturally leads to creating a key hierarchy structure which exposes a more logical structure for organizing encryption keys.


### Summary


In the end, this proved a very useful approach. Our main evidence is the amount of keys we’ve created since we started using this library. We now manage close to 1000[Tink](https://developers.google.com/tink) encryption keys being used by 300 services and workloads. Lastly, the amount of data being encrypted on a daily basis has steadily increased to 18TB.
