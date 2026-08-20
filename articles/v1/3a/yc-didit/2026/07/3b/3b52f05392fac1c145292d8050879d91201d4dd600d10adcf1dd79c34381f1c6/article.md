---
schema_version: "1.0.0"
document_id: "3b52f05392fac1c145292d8050879d91201d4dd600d10adcf1dd79c34381f1c6"
company_key: "yc-didit"
company: "Didit"
source_id: "yc-didit-news-import-d47711ec305c"
canonical_url: "https://didit.me/blog/mrz-machine-readable-zone-technical-guide/"
published_at: "2026-07-28T18:39:37.640+00:00"
first_seen_at: "2026-07-29T00:27:47.387739+00:00"
fetched_at: "2026-07-29T00:27:49.458663+00:00"
content_hash: "sha256:6728eff169ca2f5616dbef5cd2715f6cdb3c8091fcb23fa08fd5d00c838b2c8e"
---

# MRZ Explained: Machine Readable Zone Technical Guide

[Back to blog](https://didit.me/blog/) Blog · July 28, 2026


# MRZ Explained: Machine Readable Zone Technical Guide


A technical guide to the Machine Readable Zone: ICAO 9303 TD1, TD2, and TD3 layouts, check digits, parsing pitfalls, and its relationship to NFC chips.


By Didit


·


July 28, 2026 ·


Updated Jul 28, 2026


MRZ means **Machine Readable Zone** : the standardized block of fixed-width characters printed on passports, identity cards, and other machine-readable travel documents.


It encodes selected identity and document fields for optical character recognition. Check digits help detect reading or transcription errors. They do not prove that the document is genuine, that a trusted authority issued the data, or that the presenter is the rightful holder.


## **Key takeaways**


- **MRZ is a family of layouts, not one universal string.** TD1 uses three lines of 30 characters, TD2 uses two lines of 36, and TD3 uses two lines of 44.
- **Positions carry meaning.** A parser must preserve line boundaries, field widths, filler characters, and the schema selected for that document.
- **Check digits detect errors; they are not signatures.** ICAO’s modulus-10 calculation uses character values and repeating weights of 7, 3, and 1.
- **Names and dates need policy, not guesswork.** Transliteration, truncation, fillers, and two-digit years can make a syntactically valid parse semantically uncertain.
- **MRZ and NFC serve different trust functions.** MRZ is optically readable data; an authenticated ePassport chip can provide digitally signed data and optional anti-cloning evidence.


## **What is an MRZ?**


The Machine Readable Zone is the mandatory machine-readable area of an ICAO-compliant machine-readable travel document. It appears separately from the **Visual Inspection Zone (VIZ)** , where a person sees labels, full dates, a portrait, and other document details.


[ICAO Doc 9303](https://www.icao.int/publications/doc-series/doc-9303) defines the travel-document system across multiple parts. Part 3 contains specifications common to machine-readable travel documents. Parts 4, 5, and 6 define the TD3, TD1, and TD2 layouts respectively. The layouts use OCR-B characters and a deliberately restricted repertoire:


- uppercase letters` A` through` Z` ;
- digits` 0` through` 9` ;
- the filler character` <` .


The filler is not decorative. It pads unused fixed-width fields, replaces spaces or unsupported punctuation where specified, and separates name components. Removing every` <` before parsing destroys information about field boundaries and names.


The MRZ repeats selected document data. It is optimized for interoperable capture, not for preserving every visual field or national-script name.


## **TD1, TD2, and TD3 formats compared**


The first parsing decision is the document layout. Line count and exact width provide the strongest initial discriminator.


Format Typical form factor MRZ shape Total characters Name location


**TD1** Credit-card-size official travel document or identity card 3 × 30 90 Line 3


**TD2** Larger card or booklet data page 2 × 36 72 Line 1


**TD3** Machine-readable passport and other TD3 documents 2 × 44 88 Line 1


Do not infer the format only from a document code such as` P` or` I` . Capture can omit a line, join two lines, insert spaces, or return a crop from the wrong region. Validate both geometry and permitted characters before assigning field positions.


## **TD1: three lines of 30 characters**


[Doc 9303 Part 5](https://www.icao.int/sites/default/files/publications/DocSeries/9303_p5_cons_en.pdf) defines TD1-size machine-readable official travel documents. Because the card is compact, the MRZ spans three lines.


### **TD1 line 1: document and optional data**


Positions Length Field


1–2 2 Document code


3–5 3 Issuing State or organization


6–14 9 Document number


15 1 Document-number check digit


16–30 15 Optional data


The first character of the document code is` A` ,` C` , or` I` under the Part 5 layout. Unused optional positions are filled with` <` .


For a document number longer than nine characters, upper-line position 15 is` <` . Continuation characters use positions 16 through at most 28; the number's check digit immediately follows in positions 17–29, and a filler follows that digit. Parse this exception before treating positions 16–30 as optional data.


### **TD1 line 2: holder attributes and composite check**


Positions Length Field


1–6 6 Date of birth in` YYMMDD` form


7 1 Birth-date check digit


8 1 Sex:` F` ,` M` , or \`<\`


9–14 6 Date of expiry in` YYMMDD` form


15 1 Expiry-date check digit


16–18 3 Nationality


19–29 11 Optional data


30 1 Composite check digit


The composite digit covers upper-line positions 6–30, then middle-line positions 1–7, 9–15, and 19–29. It excludes the holder's name on line 3. Concatenate these exact ranges in order.


### **TD1 line 3: name**


All 30 positions hold the name. Two fillers,` <<` , separate the primary identifier—usually the surname or surnames—from secondary identifiers. A single` <` separates components within either group.


For example:


```text
ERIKSSON<<ANNA<MARIA<<<<<<<<<<
```


This represents the primary identifier` ERIKSSON` and secondary identifiers` ANNA MARIA` , followed by padding. Use the issuer’s identifier groups instead of assuming that every culture uses one surname followed by given names.


## **TD2: two lines of 36 characters**


[Doc 9303 Part 6](https://www.icao.int/sites/default/files/publications/DocSeries/9303_p6_cons_en.pdf) defines the TD2 layout.


### **TD2 line 1**


Positions Length Field


1–2 2 Document code


3–5 3 Issuing State or organization


6–36 31 Name


As in TD1, the first document-code character is` A` ,` C` , or` I` . The name follows the same primary-identifier,` <<` , secondary-identifier structure, but its available length differs.


### **TD2 line 2**


Positions Length Field


1–9 9 Document number


10 1 Document-number check digit


11–13 3 Nationality


14–19 6 Date of birth


20 1 Birth-date check digit


21 1 Sex


22–27 6 Date of expiry


28 1 Expiry-date check digit


29–35 7 Optional data


36 1 Composite check digit


The composite digit covers positions 1–10, 14–20, and 22–35. Nationality positions 11–13 and sex position 21 are excluded. That exclusion is a common source of “almost correct” implementations.


For a document number longer than nine characters, positions 1–9 hold its nine principal characters, position 10 is` <` , and the remaining characters start in positions 29–35. The document-number check digit follows the final continuation character, then a filler. Apply this exception before interpreting positions 29–35 solely as optional data.


## **TD3: two lines of 44 characters**


[Doc 9303 Part 4](https://www.icao.int/sites/default/files/publications/DocSeries/9303_p4_cons_en.pdf) defines machine-readable passports and other TD3-size documents.


### **TD3 line 1**


Positions Length Field


1–2 2 Document code


3–5 3 Issuing State or organization


6–44 39 Name


For a machine-readable passport, the first document-code character is` P` . The second character identifies a passport type under the applicable Part 4 rules. Name components use the same filler-based structure as TD1 and TD2.


### **TD3 line 2**


Positions Length Field


1–9 9 Passport number


10 1 Passport-number check digit


11–13 3 Nationality


14–19 6 Date of birth


20 1 Birth-date check digit


21 1 Sex


22–27 6 Date of expiry


28 1 Expiry-date check digit


29–42 14 Personal number or optional data


43 1 Optional-field check digit


44 1 Composite check digit


When the optional personal-number field is unused and filled with` <` , Part 4 allows its check position to be` 0` or` <` at the issuer’s option. A validator should accept the permitted variant without weakening checks on fields that are populated.


The final composite digit covers positions 1–10, 14–20, and 22–43. It therefore includes the individual check digits in those ranges but excludes nationality and sex.


## **How MRZ check digits work**


ICAO check digits use a deterministic modulus-10 calculation.


### **1. Convert each character to a value**


Character Value


` 0` –` 9` 0–9


` A` –` Z` 10–35


\`<\` 0


### **2. Apply repeating weights**


Starting with the first character in the protected field, multiply values by:


```text
7, 3, 1, 7, 3, 1, ...
```


### **3. Sum and take the remainder**


Add the products and calculate the sum modulo 10. The remainder, from` 0` to` 9` , is the check digit.


For the sample passport number` L898902C3` :


```text
Characters: L   8   9   8   9   0   2   C   3
Values:    21   8   9   8   9   0   2  12   3
Weights:    7   3   1   7   3   1   7   3   1
Products: 147  24   9  56  27   0  14  36   3
Sum: 316
316 mod 10 = 6
```


The expected check digit is` 6` .


Validate individual digits and the composite digit. An individual failure identifies a likely field-level problem; a composite-only failure can indicate an error in optional data, range construction, or another covered character.


Check digits catch many substitutions and transpositions, but an attacker can recompute them. They provide error detection, not authenticity against forgery.


## **Parsing pitfalls that break production systems**


### **Losing fixed-width structure**


Trimming each line, collapsing` <` , or deleting line breaks before format detection shifts fields. Normalize outer whitespace only after isolating the MRZ, then require exactly 30, 36, or 44 characters per line.


### **Treating OCR output as ground truth**


OCR commonly confuses` O` with` 0` ,` I` with` 1` , and` B` with` 8` . Do not apply a global replacement: letters are valid in document numbers, and digits are valid in dates. Use the field’s permitted character set, check digits, the VIZ, and another capture to guide recovery. Record both the raw OCR result and any correction.


### **Removing fillers too early**


` <` can mean padding, a separator, an unspecified value, or a replacement required by the schema. Parse positions first. Interpret fillers only within the selected field.


### **Assuming the MRZ preserves the printed name**


The MRZ omits punctuation, transliterates national characters, and can truncate names to fit.` <<` separates identifier groups;` <` within a group separates components. Store the raw MRZ name and the document’s visual or authoritative name separately rather than silently overwriting one with the other.


An alphabetic character in the last available name position signals possible truncation. A name can exactly fill the field and trigger the same signal, so treat it as uncertainty.


### **Inferring the wrong century**


MRZ birth and expiry dates use two-digit years. The check digit validates the six printed digits but does not choose a century. Resolve the full date with document type, expiry rules, capture date, holder context, and explicit policy. Keep the raw` YYMMDD` value so a later rule change remains auditable.


### **Treating three-letter codes as a generic country library lookup**


Issuing authority and nationality use the code set specified by Doc 9303, which includes organizations and special values as well as States. A strict ISO-only mapping can reject valid documents or mislabel the issuer. Version and test the ICAO code table used by the parser.


### **Ignoring issuer options and long document numbers**


Optional data is issuer-defined, and long document-number rules can relocate continuation characters and a check digit. Do not assign a universal meaning to every optional field or concatenate it into the document number without format-specific evidence.


### **“Repairing” a failed check digit silently**


A failed digit may come from glare, crop, a damaged line, an unsupported issuer convention, or manipulation. Preserve the raw capture, identify the failed field, and request a controlled recapture or review. Silent correction erases the evidence needed to understand the failure.


## **MRZ versus NFC chip data**


MRZ and NFC are complementary, not competing versions of the same check.


Property Printed MRZ Contactless ePassport or eID chip


Capture Camera, scanner, or manual transcription NFC-capable reader


Data Selected fixed-width identity and document fields Structured data groups, potentially including a higher-quality portrait


Error control Modulus-10 check digits Data-group hashes and digital signatures when authenticated


Authenticity Not established by the MRZ alone Passive Authentication can establish issuing-authority signature and data integrity


Anti-cloning None Active Authentication or Chip Authentication may provide evidence when supported


Availability Broad across machine-readable travel documents Only documents with a supported contactless chip and readable device path


The MRZ can also participate in chip access. ICAO’s[ePassport access-control guidance](https://www.icao.int/icao-pkd/epassport-validation-roadmap-tool-document-readers) explains that Basic Access Control derives an access key from data read from the passport data page, normally through the MRZ. PACE is a stronger access-control protocol encountered in newer documents. Access control protects chip communication; it is not the same as authenticating the issuer’s signature.


After access,[Passive Authentication](https://www.icao.int/icao-pkd/epassport-validation-roadmap-tool-system-requirements) checks the digital signature so the inspection system can determine whether the chip data was saved by the issuing authority and altered later. Optional active or chip-authentication mechanisms can add anti-cloning evidence.


A robust document flow can therefore:


1. capture the MRZ and validate its structure and check digits;
2. use the required access data to establish a protected NFC session;
3. read the chip data groups;
4. perform Passive Authentication against trusted issuer certificates;
5. perform an anti-cloning mechanism when the document supports it;
6. compare chip, MRZ, and visible document data;
7. separately verify that the applicant is linked to the document.


Reading a chip without validating its signature is not equivalent to authenticating it. Likewise, a valid chip does not by itself prove that the current applicant is its rightful holder.


## **Implementation and security checklist**


Before shipping an MRZ parser:


- detect TD1, TD2, or TD3 from exact line geometry before slicing fields;
- preserve the raw image, raw OCR lines, normalized lines, and parser version;
- accept only the permitted MRZ character repertoire after controlled normalization;
- parse by one-based ICAO positions translated carefully into the programming language’s indexing;
- validate every applicable field digit and the format-specific composite digit;
- support issuer-permitted empty-field and long-document-number cases;
- retain raw and interpreted dates, codes, names, and optional data separately;
- surface truncation and century inference as explicit uncertainty;
- compare MRZ results with VIZ or chip data without silently overwriting mismatches;
- avoid placing MRZ strings in analytics, URLs, crash reports, or routine logs;
- encrypt sensitive retained data and apply access, retention, and deletion policy;
- test genuine samples across issuers, formats, long names, damaged captures, and OCR confusions;
- fuzz line lengths, fillers, invalid characters, and check-digit failures;
- keep document acceptance policy separate from parsing success.


A parser answers, “What characters are encoded, and are the check digits consistent?” Document authentication and holder verification require additional evidence.


## **Where Didit fits**


Didit lists[ID Verification](https://didit.me/products/id-verification) and[NFC Reading](https://didit.me/products/nfc-verification/) as separate document-and-identity modules. Their published prices are **$0.15 each** . The[Workflow Orchestrator](https://didit.me/products/workflow-orchestrator) is listed as free.


Those product links do not change the trust distinction in this guide: MRZ parsing, chip authentication, document policy, and holder linkage remain separate conclusions. Current module rates are available on the[pricing page](https://didit.me/pricing) .


## **Frequently asked questions**


### **What does MRZ stand for?**


MRZ stands for Machine Readable Zone, the standardized fixed-width character area printed on machine-readable travel documents.


### **Where is the MRZ on a passport?**


On a TD3 passport data page, it is the two-line block of 44 characters per line, normally at the bottom of the data page.


### **What is the difference between TD1, TD2, and TD3?**


They are ICAO document sizes and MRZ layouts. TD1 has three lines of 30 characters, TD2 has two of 36, and TD3 has two of 44.


### **Can an MRZ check digit prove a passport is genuine?**


No. It detects many accidental reading errors. It is not a digital signature and can be recomputed after data is changed.


### **Why does an MRZ use the` <` character?**


It fills unused positions, replaces spaces or unsupported punctuation where required, separates name components, and can represent an unspecified field in defined contexts.


### **Is the MRZ the same data as the NFC chip?**


No. Some identity fields overlap, but the chip can store structured data groups and cryptographic security objects. Proper chip authentication can provide evidence that the signed data came from the issuing authority and was not altered.


### **Should an application trust OCR-corrected MRZ data?**


Only with explicit evidence. Corrections should be field-aware, supported by check digits or another source, and retained alongside the raw OCR result. Unresolved inconsistencies should trigger recapture or review.


## **Primary references**


- [ICAO Doc 9303 series: Machine Readable Travel Documents](https://www.icao.int/publications/doc-series/doc-9303)
- [ICAO Doc 9303 Part 3: Specifications Common to all MRTDs](https://www.icao.int/sites/default/files/publications/DocSeries/9303_p3_cons_en.pdf)
- [ICAO Doc 9303 Part 4: Machine Readable Passports and other TD3 Size MRTDs](https://www.icao.int/sites/default/files/publications/DocSeries/9303_p4_cons_en.pdf)
- [ICAO Doc 9303 Part 5: TD1 Size Machine Readable Official Travel Documents](https://www.icao.int/sites/default/files/publications/DocSeries/9303_p5_cons_en.pdf)
- [ICAO Doc 9303 Part 6: TD2 Size Machine Readable Official Travel Documents](https://www.icao.int/sites/default/files/publications/DocSeries/9303_p6_cons_en.pdf)
- [ICAO ePassport Validation Roadmap: Access to the ePassport chip](https://www.icao.int/icao-pkd/epassport-validation-roadmap-tool-document-readers)
- [ICAO ePassport Validation Roadmap: Inspection system requirements](https://www.icao.int/icao-pkd/epassport-validation-roadmap-tool-system-requirements)


## **Conclusion**


The MRZ succeeds because it is constrained: fixed line lengths, fixed positions, a limited character set, and predictable check digits. Those same constraints demand disciplined parsing. Keep raw structure intact, select the correct TD layout, calculate exact protected ranges, and preserve uncertainty around names, dates, and issuer-specific options.


Then stop at the boundary of what the evidence proves. A valid MRZ is useful interoperable data. Authenticated NFC chip data can add cryptographic document evidence. Holder verification and the final business decision remain separate steps.


Keep reading


## Related articles


- [PEP Screening: Definitions, Scope, and Monitoring](https://didit.me/blog/pep-screening-definitions-scope-monitoring/)
- [Enhanced Due Diligence (EDD): Compliance Guide](https://didit.me/blog/enhanced-due-diligence-edd-compliance-guide/)
- [Age Verification Software: Buyer's Guide](https://didit.me/blog/age-verification-software-buyers-guide/)
- [Flutter SDK: Add Identity Verification to Your App](https://didit.me/blog/flutter-sdk-identity-verification-integration-guide/)
- [W3C Decentralized Identifiers (DIDs) Specification](https://didit.me/blog/w3c-decentralized-identifiers-dids-specification/)
- [Adverse Media Screening: Process, Tuning, and Risks](https://didit.me/blog/adverse-media-screening-process-tuning-guide/)
