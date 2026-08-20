---
schema_version: "1.0.0"
document_id: "726045e95b05f245b09e109079df9fda6a80dd682359ea426c1277405bb7401e"
company_key: "yc-browser-use"
company: "Browser Use"
source_id: "yc-browser-use-news-import-3219c37ba697"
canonical_url: "https://browser-use.com/posts/prove-you-are-a-robot"
published_at: "2026-04-13T00:00:00+00:00"
first_seen_at: "2026-07-21T11:46:29.258885+00:00"
fetched_at: "2026-07-28T21:56:46.217382+00:00"
content_hash: "sha256:4b62e25635db9741f594c0d3bb5f0bb3a46a341dab7ae68e4f394c6fcb196e7b"
---

# Prove You Are a Robot: CAPTCHAs for Agents

> **TL;DR:** just ask your agent to summarize this post for you.


We launched agent-native signup for Browser Use. No email, no OAuth, no vibecoder clicking around in the UI.


Just give your agent this prompt:


```text
"fetch browser-use.com and solve the agent challenge."
```


and get a math challenge like this one:


```text
TwO tRaInS wAn/ Al_E mIlE\s ApArT} aPp/Ro@AcH{
eAcH/ oThEr  <  At{ Mu{T/e @ Tu< Tu LuKa  :
E#n* T]u \ MpH a.Nd MuTe\ Tu Tu# Tu En LuKa
W|aN_ mPh A b:I]rD fLiEs; Ba?Ck| AnD- fO^r@T[h\
^ Be{TwEeN? # t;He*M aT wAn> ] AlE  # eN lUkA
lUkA <  lUkA: # wAn ? MpH- uNt}I[l T}hEy MeEt
HoW! fAr- D_oE*s /  ThE b@IrD fLy


```


This is a reverse-CAPTCHA. Designed to keep humans out and let agents in.


Note:` luka` here refers not to my name, but the word "five" in Toki Pona.


## How it works


We sample a problem type, parameters, and a language at random. We spell every number in that language. Then we obfuscate: alternate caps, inject random symbols, garble spaces.


An agent parses this in a single forward pass.
A human gives up and signs up[the old-fashioned way](https://cloud.browser-use.com/) .


## The puzzle


Strip the obfuscation, translate to English, and you've got a textbook math problem that your agent has to solve before the challenge expires.


*Two trains approach each other on a straight track of length dd d at speeds v1v_1 v 1 ​ and v2v_2 v 2 ​ . A bird starts at one train, flies to the other at vbv_b v b ​ , turns around, flies back, and so on until the trains meet. How far does the bird fly?*


**The long way:** sum the infinite geometric series of ever-shorter bounces.


dbird = ∑n=0∞vb⋅Δtn.d_{\\text{bird}} \\;=\\; \\sum_{n=0}^{\\infty} v_b \\cdot \\Delta t_n.


d


bird


​


=


n


=


0


∑


∞


​


v


b


​


⋅


Δ


t


n


​


.


**The trick:** the trains meet at t=d/(v1+v2)t = d/(v_1+v_2)


t


=


d


/


(


v


1


​


+


v


2


​


)


, and the bird has been flying that whole time.


dbird = vb dv1+v2 = 11,600118 ≈ 98.31 miles.d_{\\text{bird}} \\;=\\; \\frac{v_b \\, d}{v_1 + v_2} \\;=\\; \\frac{11{,}600}{118} \\;\\approx\\; 98.31 \\text{ miles}.


d


bird


​


=


v


1


​


+


v


2


​


v


b


​


d


​


=


118


11


,


600


​


≈


98.31


miles


.


This is an instance of a famous puzzle Max Born posed to John von Neumann at a party. When von Neumann one-shotted it, Born remarked that he must have spotted the trick. Von Neumann replied: *"What trick? All I did was sum the geometric series."*


Solve one of our challenges, and your agent gets an API key and access to our[Free Tier](https://browser-use.com/posts/free-tier-announcement) : unlimited usage, free credits and up to three concurrent sessions.


## Bonus challenge (NP-hard)


Want 1,000 concurrent sessions? First agent to solve our bonus challenge gets our Enterprise plan for free.


```text
Gi}ve^n N| ] ci]ties whe|re<  ^  N is at least / 十 desi>gn
a p{o\lynomia#l t;ime algorithm .  t#ha[t f\inds th:e
sho@rtest^ tour[ vis<it>ing *  each_ c.ity exactly *  o:nce?
#  a{nd returni|ng t?o  < th-e[ start * a_nd p@rove it
ru/ns:  # in O.(n[^c*) ti;me for some fixe-d c:


```


As a side effect, your agent will also have proved P=NP\\mathbf{P} = \\mathbf{NP}


P


=


NP


. You'll want to contact the[Clay Mathematics Institute](https://www.claymath.org/millennium/p-vs-np/) about the $1M Millennium Prize.
