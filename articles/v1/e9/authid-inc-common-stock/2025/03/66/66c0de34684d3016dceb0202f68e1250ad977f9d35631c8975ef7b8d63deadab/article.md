---
schema_version: "1.0.0"
document_id: "66c0de34684d3016dceb0202f68e1250ad977f9d35631c8975ef7b8d63deadab"
company_key: "authid-inc-common-stock"
company: "authID Inc."
source_id: "authid-inc-common-stock-rss-c1a5aa3dd4ef"
canonical_url: "https://authid.ai/liveness-detection/"
published_at: "2025-03-11T04:57:29+00:00"
first_seen_at: "2026-07-24T08:14:02.330532+00:00"
fetched_at: "2026-07-28T20:58:14.920102+00:00"
content_hash: "sha256:73e1d53b3a17b68308072869d71427e63a97be2bdf834a2c401c248f31fd50d7"
---

# Are You Alive, or Something Else?

To our friends, family, the people who share the elevator with us, we’re real people, flesh and blood. We’re 3D, clearly here and present. We have depth and breadth. We’re alive. And to make ourselves known, we say “hello” or we reach out for a handshake.


But in the digital world, we’re not even 2D. We’re a thin stream of bytes transmitted over a very narrow pipe. To make ourselves known, we push those bytes through that pipe and hope that they’re enough for the equivalent of that handshake.


Why do we do this? On what’s sometimes called Day Zero, when we want to sign up for an account, a credit card, a loan, a whatever, we have to establish our identity. Then on Day One and beyond, when we want to return to take advantage of whatever we’ve signed up for, we assert that identity, to be validated against that Day Zero registration. The site we visit says, yes, that’s you again, welcome back.


The problem there is that whoever’s on the other end of that pipe has to trust that those bytes really add up to a legit identity, a real person, on Day Zero. To make it even more complicated, even if they trust our bytes that first time, they can’t trust that the next time they see those bytes, it’s still us, and not somebody who’s hijacked our bytes, pretending to be us on return visits.


So what do we package up in those bytes to establish, and later reassert, our identities?


PII was meant to represent us more deeply, reflecting personal factors that only we would know. Email addresses, phone numbers, social security numbers, and other information is presumed to be useful, but the constant parade of breaches have put all that data out there. This means that any criminal, foreign agent, or bot can provide that data when applying for accounts, credit cards or loans in our names. So what’s next?


Our devices. They’re in our hands. Nobody else can be me if they don’t have the phone that’s in my physical possession. Oh, but devices can be stolen. They can be hijacked, the card can be swapped. Hey, I’ll bind it to my face, my voice, my fingerprint. But a thief can acquire a device put their own face, voice, fingerprint on it.


On return visits, we might use passwords. But passwords are more bare than a bare minimum. Their value has been degraded by malware, brute force, social media, social engineering, our own laziness in composing them.


So now the people on the other side of the pipe can’t trust what’s in our head or what’s in our hand. What’s next?


Who we are, in the form of biometrics, meaning digitally-acquired physical aspects of our persons.


If we must present physical aspects from our actual bodies when digitally identifying ourselves, it doesn’t matter if somebody else has our data or our devices, right? We have to produce those bodily factors from our own selves every time, regardless of the device. We produce our voices, our fingerprints, our faces. Those biometric signals are then evaluated by the requesting platform.


But it’s still not that easy, or secure. Voice cloning makes it far too easy for others to sound like us. There are already many documented case of criminals capturing and reproducing voices to fool colleagues and, more frightening, family members. Meanwhile, the most ubiquitous personal device manufacturer has decided that supporting fingerprints was no longer on its roadmap.


Facial recognition has been around a long time. But even that can be fooled on many digital platforms. Pictures, printouts, screen replays, even primitive masks can mimic us online. A majority of people say they can identify a deepfake when they see it, but a quick scan of Facebook, where visitors regularly fall for (and praise) AI-generated images, tells us otherwise.


So what’s next after next and the next-next?


Facial biometrics are still the way to go. But like the magic trick that everybody already knows the secret to, it’s not what you do, it’s how you do it. The key to properly, securely, confidently identifying an individual by way of their face in the digital world is employing facial liveness detection.


## What is Liveness Detection?


*Liveliness* is a word that usually describes how much fun we are at parties. But you don’t identify yourself by putting a lampshade on your head. *Liveness* indicates that you are, clearly enough, alive. In fact, with proper biometric liveness detection, you don’t really have to do much except put yourself in front of a camera. Liveness then does all the work. So what is liveness detection, precisely?


First off, here’s what liveness is not. Device-bound solutions such as FaceId are absolutely not facial liveness detection. They look at the math of your face, the distance between your eyes, nose, lips, and it’s not even decent algebra and try to make sure that it’s really you coming back, assuming you were even properly registered with that face in the first place (and once again, a purloined device can be used against you when the thief installs their own face). Further assuming that you’re being evaluated by a digital entity rather than a person, a mask that looks nothing like you but which matches the geometry of your face can fool such a mechanism.


Liveness isn’t the same as facial recognition, which simply matches one face to another, or searches for that face in a database of other faces using static image files. Liveness detection determines that the submission is that of a live person, an actual human being, in real time.


Liveness is the quality or state of being alive, as measured by the observation of factors presented by an actual human being. Biometric liveness detection, the measuring of true liveness, is how providers determine if the applicant, requester, or transaction initiator is a real person, in real time.


## How Liveness Works


Video fakes can mimic movements, obviously. But with AI, even still shots can be animated to mimic movements. Liveness detection therefore requires more than simply noting motion. Here is how liveness works.


Liveness detection figures out that you’re alive. You are exhibiting liveness, even when not in active motion. Voluntary or involuntary actions or reactions, texture, depth, and other aspects of who is in front of the camera all contribute to face liveness detection.


Presentation Attack Detection (PAD) is the automated determination of whether a non-live entity is being presented to the camera. This is commonly called a presentation attack. Facial liveness detection evaluates whether the facial biometric being captured by the camera is being presented by a live subject.


The standard for facial liveness detection is NIST (national Institute for Standards and Technology). Specifically, there is the standard codified by ISO (30107-3 Level 1 and 2 standards for Presentation Attack Detection) that specifies an organization’s ability to enforce the standard and provide protection from presentation fakes. That ability to enforcement a level of liveness detection is typically certified by third party testing.


Static images, such as picture files, cannot present natural, inherent, involuntary motions that a live person, even when holding still in front of a camera, still produces. Liveness notes such movement and incorporates it into its decision-making.


Static images also cannot produce the depth that liveness detection picks up. Liveness detection essentially evaluates the 3D aspects of what is presented to the camera.


Texture is another factor that facial liveness detection understands. A photo, even when reproduced digitally to the camera, will not appear to liveness detection as a live image.


In the end, facial liveness detection separates live people from anything else that attempts to reproduce a person. It is not so much about identifying what the image is so much as what it is not. Either it’s a live human, or it’s something else, whatever it is. And if that person isn’t live, it doesn’t get it. That’s how liveness works: it makes a binary decision.


That said, deeper facial liveness detection solutions can provide some level of scoring about an image, reporting on what precisely it did not like about any image it rejects, giving an organization more data about how a decision was rendered.


## Types of Liveness Detection


Even though there are standards for facial liveness detection, there exist multiple approaches to providing it. Face liveness detection is made up of various types of algorithms for evaluating the efficacy of a captured facial image. These approaches largely fall under two categories, active liveness and passive liveness.


What is *active liveness* ? Simply enough, it requires the subject to move. This movement usually involves blinking, smiling, nodding, even speaking. Intuitively, this means the person is live. Look, they’re moving, so it must be a real person. But active liveness is not as welcome, nor as effective, as a cursory look might indicate.


First, many individuals find the need to mug for the camera as uncomfortable, especially in public. It takes longer, and can fail as often as not. Even after blinking, smiling, or spinning one’s head around, the person may be rejected. It can be more theater than true fraud detection. Therefore it can be a barrier to adoption.


Second, active liveness detection can be fooled. As noted earlier, video replays as well as AI-animated photos can demonstrate those exact same motions. Free websites allow users to upload photos for animation. For that matter, these sites can even morph existing faces to more closely resemble someone they’re not. So why subject users to what is considered a needless, invasive process when it provides no additional value?


The alternative is *passive face liveness detection* . This requires no movement, or at least no voluntary movement. There is no instruction set regarding blinking, smiling, or other activity. The user only needs to pose for a picture. That does not mean that movement does not happen. More on that in a moment.


Companies increasingly prefer utilizing passive face liveness detection. It is faster, since it doesn’t require active participation beyond taking a picture. The individual is not even aware of what’s going on beyond that image capture, and so it represents a smoother user experience. It is also not obvious to any observers that a passive liveness detection process is even taking place.


In the absence of active movement, passive liveness detection detects features of presentation attacks such as edges, texture, depth, and micro-movements to clearly distinguish the face of a living person from a lifeless or fake face. Passive facial liveness detection (from capable vendors) is not easily fooled by animation software that mimics facial expressions such as smiling or frowning. It can also handle more stringent presentation attacks such as the use of deepfakes, 3D masks, puppets and so on.


Since passive liveness detection requires no voluntary movement, the process takes far less time, and certainly no instructions. The user does not even need to hold totally still. Because it is a simpler experience, there are far fewer users abandoning such a process.


Passive liveness doesn’t mean simply taking a single shot. Passive liveness detection often entails capturing multiple shots to measure micro-movements, then selecting the best frame for subsequent authentication purposes. That authentication is enabled by the original liveness detection which establishes a legit identity. Logging in by way of one’s face is now possible because follow-on requests are also subject to liveness detection. “The live person asking for a session this time is the same live person who registered yesterday.”


As mentioned earlier, while the user is not required to actively move, passive liveness detection still notes micro-movements and involuntary motion, in addition to all the other possible artifacts that indicate a live person versus a fake, deepfake, mask, or other fraudulent mechanism.


*Hybrid liveness* is the blend of these two approaches, active and passive. Hybrid liveness means the user isn’t necessarily blinking or moving, but maybe only smiling. But again, this is nothing that AI-animated images can’t reproduce, rendering hybrid liveness largely moot.


## Additional Liveness Use Cases


Liveness detection is not just for preventing fake, modified, fabricated, or deepfake faces. User onboarding processes often involve the presentation of physical identification documents such as drivers licenses, state or provincial ids, passports, or other artifacts. Therefore liveness can be applied in determining that such an identification document being presented is not a screen replay, picture, printout, or some item other than an id held in a live person’s hand in real time.


N.B. This will produce an ongoing challenge for the use of mobile drivers licenses online.


Here’s another example of an outlier. It’s not uncommon for an unqualified candidate to find a stand-in to take an actual online video interview on their behalf, someone who can actually handle the questions. Then the unqualified one shows up for the job. Face liveness detection can be used to replay the original identification to verify the person who physically shows up on the first day.


As previously stated, after liveness has established that an applicant is a legit, live person and is now registered, continuous authentication entails liveness identification. This means that the user is asking for access to digital assets to which they are entitled because liveness previously said they were good to go. Now they demonstrate they are still a live human, and not a fake trying to fool the system in order to steal their access.


## Combatting Deepfakes With Liveness


Deepfakes, or AI-fabricated images or videos that are either wholly fake or morphed from a base photo, are already dangerously realistic and will only get better as the technology continues to improve itself through machine learning and adversarial networks. Many systems are still vulnerable to deepfakes, in the form of both faces and physical ids. Famously, a journalist was able to cheaply produce, via a public website, to produce a fake id that fooled a popular crypto platform. In some well-publicized crimes from 2024, two different individuals in Hong Kong were convinced to wire millions to fraudsters who employed incredibly sophisticated deepfake attacks over video conferences, mimicking the faces and voices or their colleagues to request large transactions.


Such attacks demonstrate the degree to which deepfakes have established themselves as legit, dangerous, and growing threats that must be identified when presented.


Fortunately, deepfake images present both visible and invisible artifacts that more sophisticated defenses can detect. The most capable passive liveness detection solutions can differentiate between a live person and an essentially dead deepfake image. As stated, active motion is not the same as liveness.


As a sidenote, deepfakes present a threat beyond presentation attacks directly to the camera. More talented purveyors of deepfakes will attempt to bypass liveness detection by inserting fabricated images behind the camera or normal point of capture, utilizing virtual cameras, vulnerabilities in the network, or even points where API calls submit images from client-side applications. Such efforts are called injection attacks. These are less common, since they take far more capability and knowledge of application infrastructure than the use of standard fakes and deepfakes. Injection attacks must be opposed with completely different types of defenses which verify that images being evaluated on the back end have originated from the expected capture point (i.e. camera app).


## Liveness Solutions Aren’t Created Equal


In the “old days,” teenagers would modify ids with Exacto knives and felt tip pens to gain entry to age-gated establishments. Until it was shut down, an infamous website was selling, very cheaply, fake identity documents that could defeat the identification systems at various crypto platforms. These are physical documents. Digital systems make it even easier to proliferate such fakes. Photoshop and even MSPaint are still used in modifying photos of these documents.


The depth of the algorithms employed by different vendors varies. Fakes still get by liveness checks all the time. AI-generated deepfakes are not just getting better, they’re also being generated at an industrial scale, and for free. The sheer volume of these fakes guarantee that even a small percentage of success equates to a large volume of intrusions.


Too many vendors advertise their basic liveness detection as preventing deepfakes. But deepfakes are a whole other beast, and require their own detection algorithms. For a truly secure environment, safe from fakes and deepfakes, organizations need a provider who is forward-thinking in terms of best practices for identifying legitimately live users. Ultimately, detecting fakes is simply the byproduct of what actually grows business, meaning identifying good, live candidates who help expand legitimate user stores and grow business.


## About authID, the Leader in Liveness and Deepfake Detection


authID, the leading provider of biometric identity verification and authentication solutions, employs the highest-rated algorithm for passive face liveness detection, according to NIST. Through its use of biometric liveness detection, authID onboards users on Day Zero and establishes a biometric root of trust. Then on Day One and beyond, it uses that same technology for liveness identification, meaning it doesn’t just match a face to a face, it leverages biometric liveness detection to verify the returning user to allow access. authID ensures that our customers always know who’s behind whatever device is knocking at the door.


In independent testing, authID has achieved a perfect score in ISO-level liveness detection.


Our solution looks at every aspect of an identity transaction as an opportunity. We have several chances to enable a legit applicant to access the digital assets: validate that physical id document, validate the live person, then match them together. Of course, these are also opportunities to uncover fraud. And we do all of this through a multi-layered approach.


For first-time identity verification, or proofing:


- authID prompts for photos of the physical document and the user
- We perform both client side and back-end analysis of the images to detect deepfake attempts
- We perform this analysis within 700 milliseconds, the fastest and most accurate process on the market
- We detect deepfake images via digital signing of images, and pull apart physical documents multiple ways to determine their viability
- We employ NIST PAD 2 liveness detection to determine if the selfie is provided by a live human, and prevent presentation attacks
- We stop injection attacks, meaning preventing fraudsters from inserting fake images into the process thru hardware, software, or network attacks
- And all this happens with a simplified, friendly user experience that accommodates even the least tech-savvy individual


To accommodate the various use cases across several verticals, authID provides a number of[integration capabilities](https://authid.ai/authid-resources/) that are well-documented for our customers and partners.


For day to day authentication, authID requires only a selfie, and again employs the same liveness detection, defense against presentation and injection attacks, and of course this is all done through that same fast, accurate, and user-friendly interface.


We store captured images only as long as contractual agreements require, for review in our administrative portal. If preferred (for privacy or compliance purposes), that data can be pulled down by our customers for their own storage, and deleted immediately from our cloud.


To enable the most secure authentication in the industry, we provide an unmatched one-to-one-billion matching accuracy.


By locking out deepfake authentication, authID prevents account takeover, meaning situations where existing user accounts are compromised because a thief stole something the user knew (credentials) or something the user had (device). authID’s solution is not device-bound. Biometrics are stored and evaluated in the cloud, so that a thief cannot leverage a purloined phone. This also means that the user can authenticate from any device, and is not locked out when their phone or laptop is stolen, lost, broken, or upgraded. In the event an account is actually compromised, the user again leverages their own face and can recover their access.


authID is perfect for help desks. Some major intrusions, including two multi-million-dollar events, took place in 2023 because help desks reset passwords for imposters over the phone. Rather than simply fat-finger someone’s access, the help desk sends a link to that user to initiate an authID transaction and use facial biometrics to recover their account. The help desk has helped, but the user still needs to prove their identity.


Many multi-factor authentication schemes still rely on at least one step utilizing passwords. This means that the entire process is subject to the weakest link, meaning compromised passwords that still account for the vast majority of breaches. By leveraging biometrics, organizations can eliminate that weak link while ensuring that only real, live, legitimate users are accessing their most valuable and sensitive digital assets.


There is no application download. Rather, we insert a small executable directly into the device browser at onboarding and authentication.


authID supports compliance and protects user privacy by storing no biometric data whatsoever. It achieves this through PrivacyKey, which at onboarding time generates a private / public key pair based on the facial biometric. The public key is stored, reflecting no aspect of biometrics, while the private key is discarded. During all subsequent authentications, the private key is reproduced and used for encrypting a message that can only be decrypted by the public key. If this succeeds, it validates the authentication request and allows entry.
