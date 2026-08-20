---
schema_version: "1.0.0"
document_id: "a13d5e0a2ffcc22c85135ab0de3f55a1aa539fbff9286fe32e4b468e47fbbfd9"
company_key: "yc-orthogonal"
company: "Orthogonal"
source_id: "yc-orthogonal-news-import-993ffc785022"
canonical_url: "https://www.orthogonal.com/blog/didit-identity-verification"
published_at: null
first_seen_at: "2026-07-22T07:50:11.536244+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:fe6c5dbad54b23cde31bf68b96e7294004db9f418fa00f659f8059c13f85f113"
---

# Didit: All-in-One Identity Verification for AI Agents

AI agents handling sensitive operations need identity verification. Whether onboarding users, processing payments, or accessing restricted data, agents need to confirm "you are who you say you are."


Today we're excited to highlight[Didit](https://didit.me/) , a comprehensive identity verification platform now available on Orthogonal.


## What is Didit?


Didit is an all-in-one identity platform that unifies verification, authentication, and compliance checks into a single API. Instead of stitching together multiple providers for OTP, KYC, and AML, Didit handles it all.


## Key Features


### Email & Phone OTP


Send and verify one-time passwords via email or SMS. Simple two-step flow: send the code, then verify it.


### Database Validation (KYB/KYC)


Verify identities against official government databases. Submit an ID number and get back match data including name, date of birth, and validation status.


### AML Screening


Screen individuals against global watchlists, sanctions lists, and PEP (Politically Exposed Persons) databases. Get detailed hit reports with confidence scores.


## Using Didit with Orthogonal


### Send Email OTP


` // Using @orth/sdk


// Install: npm install @orth/sdk


import


Orthogonal


from


"@orth/sdk"


;


const


orthogonal


=


new


Orthogonal


(


{


apiKey


:


process


.


env


.


ORTHOGONAL_API_KEY


,


}


)


;


// Send a verification code via email


const


output


=


await


orthogonal


.


run


(


{


api


:


"didit"


,


path


:


"/v3/email/send"


,


body


:


{


email


:


"user@example.com"


}


}


)


;


console


.


log


(


output


)


;


// Returns: { status: "Delivered" | "Undeliverable" }


`


### Verify Email OTP


` // Verify the code the user entered


const


output


=


await


orthogonal


.


run


(


{


api


:


"didit"


,


path


:


"/v3/email/check"


,


body


:


{


email


:


"user@example.com"


,


code


:


"123456"


}


}


)


;


console


.


log


(


output


)


;


// Returns: { status: "Approved" | "Declined" }


`


### Database Validation (KYC)


` // Verify identity against government databases


// Supported: ARG, BOL, BRA, CHL, COL, CRI, DOM, ECU, ESP, GTM, HND, MEX, PAN, PER, PRY, SLV, URY, VEN


const


output


=


await


orthogonal


.


run


(


{


api


:


"didit"


,


path


:


"/v3/database-validation"


,


body


:


{


issuing_state


:


"ARG"


,


validation_type


:


"one_by_one"


,


document_number


:


"12345678"


,


gender


:


"M"


}


}


)


;


console


.


log


(


output


)


;


// Returns validation status and matched data:


// {


// status: "Approved",


// database_validation: {


// first_name: "JUAN",


// last_name: "PEREZ",


// date_of_birth: "1985-03-15"


// }


// }


`


### AML Screening


` // Screen against global watchlists and PEP databases


const


output


=


await


orthogonal


.


run


(


{


api


:


"didit"


,


path


:


"/v3/aml"


,


body


:


{


full_name


:


"John Smith"


,


date_of_birth


:


"1980-01-15"


,


country


:


"US"


}


}


)


;


console


.


log


(


output


)


;


// Returns detailed hit reports with:


// - Sanctions list matches


// - PEP (Politically Exposed Persons) matches


// - Watchlist matches


// - Confidence scores for each hit


`


### Using x402 Protocol


Didit on Orthogonal supports[x402](https://www.x402.org/) for autonomous agent payments using USDC stablecoins.


` // Install: npm install x402-fetch viem


import


{


wrapFetchWithPayment


}


from


"x402-fetch"


;


import


{


privateKeyToAccount


}


from


"viem/accounts"


;


const


account


=


privateKeyToAccount


(


process


.


env


.


PRIVATE_KEY


)


;


const


fetchWithPayment


=


wrapFetchWithPayment


(


fetch


,


account


)


;


// AML screening with automatic payment


const


url


=


"https://x402.orth.sh/didit/v3/aml"


;


const


response


=


await


fetchWithPayment


(


url


,


{


method


:


"POST"


,


headers


:


{


"Content-Type"


:


"application/json"


}


,


body


:


JSON


.


stringify


(


{


full_name


:


"John Smith"


,


date_of_birth


:


"1980-01-15"


,


country


:


"US"


}


)


}


)


;


const


data


=


await


response


.


json


(


)


;


console


.


log


(


data


)


;


`


## Available Endpoints


Endpoint Method Description Price


/v3/email/send POST Send OTP code via email $0.04


/v3/email/check POST Verify email OTP code Free


/v3/phone/send POST Send OTP code via SMS $0.30


/v3/phone/check POST Verify phone OTP code Free


/v3/database-validation POST KYB/KYC against government databases $0.31


/v3/aml POST AML screening against watchlists $0.36


## Use Cases


### User Onboarding


Verify new users with email or phone OTP before granting access. Follow up with KYC for high-risk operations.


### Fintech Compliance


Meet regulatory requirements with database validation and AML screening. Didit's LATAM coverage makes it ideal for regional fintech apps.


### Fraud Prevention


Catch synthetic identities and bad actors with government database checks. AML screening flags sanctioned individuals and PEPs.


### Agent Authentication


Let your AI agents verify user identity before executing sensitive operations like transfers or account changes.


## Why Agents Need Identity Verification


As agents gain more capabilities (sending money, accessing data, making decisions), they need guardrails. Identity verification is one of the most important:


1. **Compliance** : Many operations require KYC/AML by law
2. **Fraud prevention** : Stop bad actors before they cause damage
3. **Trust** : Users trust agents that verify before acting
4. **Audit trails** : Know exactly who authorized what


## Try It Today


Sign up for Orthogonal and get $10 free credits to try Didit and dozens of other APIs. No API keys to manage, no accounts to create.


[Get Started](https://orthogonal.com/sign-up) |[View on Orthogonal](https://orthogonal.com/discover/didit) |[Didit Website](https://didit.me/)
