---
schema_version: "1.0.0"
document_id: "c835f52f46063a0f89edc3d554412569edee0a14c36553b1dc2223e376d18334"
company_key: "inseego-corp-common-stock"
company: "Inseego Corp."
source_id: "inseego-corp-common-stock-news-import-2399808c2103"
canonical_url: "https://inseego.com/resources/blog/what-is-fips-140-2-why-is-it-important/"
published_at: "2024-02-23T21:56:05+00:00"
first_seen_at: "2026-07-22T00:03:02.873259+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:b4ae5a193436e8fbbe6ea8b9526298e51f0702085d2acfb1d150bde3230dc54d"
---

# What is FIPS 140-2? Why is it important?

[FIPS 140-2](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.140-2.pdf) , Federal Information Processing Standard 140-2, is a U.S. government standard that specifies the security requirements for cryptographic modules used to protect sensitive information. It was developed by the National Institute of Standards and Technology (NIST) and was first published in 2001.


## History of the Federal Information Processing Standard (FIPS)


The[Federal Information Processing Standard (FIPS)](https://www.nist.gov/itl/fips-general-information) was established in 1977 by the U.S. government to standardize the processing and security of information and data. It was created as a response to the growing reliance on computers and the need for consistent security standards across federal agencies.


### Development and publication


FIPS 140 was first published in 1994 and was later revised and replaced by FIPS 140-1 in 1995. In 2001, FIPS 140-2 was published as the current standard. The most recent update, FIPS 140-3, was published in 2019.


### Key requirements and goals


The main goal of FIPS 140-2 is to ensure the security and integrity of sensitive information processed by cryptographic modules. It sets rigorous standards for encryption algorithms, key management, physical security, and software interfaces. The standard also aims to provide a level of assurance that the cryptographic modules have been tested and validated for security.


## FIPS 140-2 is sunset: FIPS 140-3 transition


While FIPS 140-2 has been the benchmark for cryptographic security for over two decades, it has now been officially superseded by FIPS 140-3. This is the most crucial piece of information for any organization currently evaluating cryptographic modules.


While devices validated under FIPS 140-2 remain compliant for use in existing systems, the standard is now retired for new submissions. New cryptographic module validation submissions were no longer accepted after April 1, 2022. Furthermore, the final deadline for the complete standard retirement is set for September 21, 2026, after which all FIPS 140-2 certificates will be moved to the NIST Historical List.


FIPS 140-3 builds upon its predecessor, aligning with the international standard ISO/IEC 19790:2012 and introducing several stricter requirements. For example, it includes refined physical security measures (like Environmental Failure Protection at Level 3) and requires multi-factor authentication at Level 4. Therefore, any organization planning future procurements or new product development should be targeting FIPS 140-3 validation.


For a comprehensive breakdown of the new requirements and how they compare to this original standard, please read our dedicated blog:[What is FIPS 140-3 and how does it secure sensitive data?](https://inseego.com/resources/blog/what-is-fips-140-3-and-how-does-it-secure-sensitive-data/)


Unlock lightning-fast 5G internet almost anywhere


## Key components of FIPS 140-2


### Cryptography algorithms


FIPS 140-2 requires that cryptographic algorithms used in cryptographic module operating systems must be approved by the NIST. These algorithms must provide sufficient strength to protect sensitive data from unauthorized access. The standard also specifies the use of approved algorithms, including Advanced Encryption Standard (AES), Triple Data Encryption Standard (3DES), and Secure Hash Algorithms (SHA).


### Module validation: The CMVP and CAVP process


The process of FIPS 140-2 validation is rigorous and multi-staged, providing assurance that cryptographic modules meet the standard's requirements. This is managed under the Cryptographic Module Validation Program (CMVP), a joint effort between the U.S. National Institute of Standards and Technology (NIST) and the Canadian Centre for Cyber Security (CCCS).


The validation is a two-step effort that must be performed by a NIST-accredited third-party lab:


1. **Cryptographic Algorithm Validation Program (CAVP):** This step ensures that all cryptographic algorithms used within the module (like AES or SHA) have been correctly implemented according to FIPS standards.
2. **Cryptographic Module Validation Program (CMVP):** This final step validates the cryptographic module itself—the hardware, software, or firmware—to ensure it meets all 11 specific requirements defined by the FIPS 140-2 standard.


Achieving this validation is a significant undertaking, often requiring a process that can take 9 to 12 months and involves substantial documentation and testing resources. This is why many organizations opt for commercially available solutions, like those from Inseego, that are already certified to ensure immediate compliance.


### Security levels


FIPS 140-2 has four levels of security, with each level building upon the requirements of the previous level. These levels are designed to match the level of security needed for specific applications and environments.


#### Level 1


Level 1 focuses on the basic security requirements for a cryptographic module. This level includes physical barriers to protect the module from external tampering and basic power-up self-tests.


#### Level 2


Level 2 adds role-based authentication to the requirements of level 1. Only authorized individuals can access and perform certain functions on the module.


#### Level 3


In addition to the requirements of level 2, level 3 adds measures for physical tamper-resistance. This includes the use of tamper-evident coatings and other mechanisms to protect the module from physical attacks.


#### Level 4


Level 4 involves all the requirements from the previous levels and also includes mitigation of other attacks, such as electromagnetic interference (EMI) and electromagnetic compatibility (EMC). This level is typically required for modules that handle highly sensitive data.


## Importance of FIPS 140-2


### Ensuring secure government communications


FIPS 140-2 is of utmost importance to the U.S. government as it ensures that sensitive information is protected from unauthorized access. This includes communication within federal agencies and between different levels of government.


### Protection of sensitive data


FIPS 140-2 certification is also critical for organizations that handle sensitive data, such as financial institutions, healthcare organizations, and technology companies. It ensures that their customers' information is protected from cyber attacks and breaches.


### Compliance with regulations and standards


FIPS 140-2 is not only a U.S. government standard but is also recognized globally as a benchmark for secure cryptographic modules. Compliance with FIPS is often required by various regulations and standards, such as the Payment Card Industry Data Security Standard (PCI DSS) and the Health Insurance Portability and Accountability Act (HIPAA).


### Assurance of product quality and reliability


Obtaining FIPS 140-2 certification requires rigorous testing and evaluation of cryptographic modules. This gives customers confidence in the product's quality and reliability, as it has been proven to meet stringent security requirements.


### Global recognition and trust


Cryptographic modules that are FIPS 140-2 validated are recognized and trusted not only by the U.S. government but also by international organizations. This can open up opportunities for businesses to expand their reach and enter new markets that require FIPS 140-2 compliance.


## FIPS 140-2 in Different Industries


### Government agencies & defense contractors


FIPS 140-2 validation is of particular importance to government agencies as it ensures the security of their communication and data exchange. It is a requirement for all federal government agencies to use FIPS 140-2 validated cryptographic modules.


### Financial institutions


Financial institutions, such as banks and credit card companies, are required to comply with FIPS 140-2 standards to protect sensitive financial data and payment transactions.


### Healthcare organizations


Healthcare organizations are responsible for protecting the privacy and security of patient information, making FIPS 140-2 compliance essential. It is also a requirement for HIPAA compliance.


### Technology companies


Many technology companies use FIPS 140-2 validated cryptographic modules in their products to provide secure data communication and storage for their customers. This is especially important for software used in government and financial industries.


## Inseego can help your business be in FIPS 140-2 compliance


Inseego offers a range of secure devices that are all FIPS 140-2 certified through our[software cryptographic module](https://csrc.nist.gov/projects/cryptographic-module-validation-program/certificate/4522) and can help businesses meet FIPS 140-2’s critical security parameters. To comply with FIPS 140-2, businesses need to ensure that all their cryptographic modules (such as encryption and decryption devices) meet certain security requirements. Inseego can help businesses achieve this through the following:


**1. FIPS 140-2 compliant devices:** Inseego offers devices in their FWA 5G portfolio that are certified to be FIPS 140-2 compliant. This means that they have undergone thorough testing and evaluation to ensure that they meet the strict security requirements set by the standard.


**2. Secure communication:** Inseego devices are equipped with integrated security features, including VPN (Virtual Private Network), and other enterprise-grade security features. These capabilities are crucial for ensuring compliance with security standards. Inseego's devices and cloud management software ensure that all data transmission is encrypted and secure, providing an additional layer of protection for sensitive information.


**3. Secure management software:** Inseego devices also come with secure management software, such as remote firmware updates, access control, and secure configuration interfaces, which are important for maintaining the security of the devices. This helps businesses stay within FIPS 140-2 compliance requirements for secure device management.


**4. Ongoing support and maintenance:** Inseego provides ongoing support and maintenance for its devices, which includes regular updates and security patches. This ensures that the devices remain FIPS 140-2 compliant and help businesses stay up-to-date with the latest security standards.


By using Inseego's secure devices and services, businesses can meet the strict security requirements of FIPS 140-2 and ensure that their cryptographic modules are secure and compliant. This not only helps businesses meet government regulations but also improves the overall security of their information systems.
