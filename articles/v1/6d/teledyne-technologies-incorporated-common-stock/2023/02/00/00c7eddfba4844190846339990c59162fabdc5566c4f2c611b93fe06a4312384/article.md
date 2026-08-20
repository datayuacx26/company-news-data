---
schema_version: "1.0.0"
document_id: "00c7eddfba4844190846339990c59162fabdc5566c4f2c611b93fe06a4312384"
company_key: "teledyne-technologies-incorporated-common-stock"
company: "Teledyne Technologies Incorporated"
source_id: "teledyne-technologies-incorporated-common-stock-rss-9000605d05af"
canonical_url: "https://blog.teledynelecroy.com/2023/02/removing-oscilloscope-noise-from-pcie.html"
published_at: "2023-02-08T14:49:00+00:00"
first_seen_at: "2026-07-20T04:36:13.437201+00:00"
fetched_at: "2026-08-20T01:30:31.660362+00:00"
content_hash: "sha256:d579b579cd3cb5fbeaaa35319c8f79dc27e753b66241bbd69801de5c49eb00d6"
---

# Removing Oscilloscope Noise from PCIe 6.0 Compliance Pattern Measurements

Figure 1. The new SDAIII-PCIE6 option offers
three methods for removing oscilloscope noise
from PCIe 6.0 Compliance Pattern measurements
as required by the standard.


The new[SDAIII-PAMx](https://teledynelecroy.com/options/productseries.aspx?mseries=691&groupid=103) and[SDAIII-PCIE6](https://teledynelecroy.com/options/productseries.aspx?mseries=692&groupid=103) options for Teledyne LeCroy oscilloscopes enable you to quickly make new PCIe 6.0 noise measurements SNDR and RLM with the oscilloscope baseline noise removed, as required by the standard.


Here's a brief description of the three, proprietary noise removal methods from which you can choose.


### Manual Method


Manual uses the specified amount of oscilloscope noise for the 𝜎


scope


variable in the SNDRnr formula (described in the last post). This method is useful if you have previously measured your oscilloscope baseline noise and know what value to enter.


### Baseline Method


Baseline saves a reference of the input terminated into 50 𝝮, which is then compared to the unterminated input to determine the oscilloscope's intrinsic noise floor. The calculated oscilloscope noise is then computed into the SNDRnr measurement results. To use this method, you will save a Single acquisition of the unequalized Q0 signal, then connect the signal to a pair of 50 𝝮 terminators and measure again.


### Attenuator Method


The Attenuator method is the most accurate noise compensation method, since a signal is being applied to the oscilloscope for both reference and measurement. It compares the SNR of a full-scale signal to the SNR of an attenuated signal (at the same full-scale input range) to calculate the oscilloscope's noise contribution to the SNDR as per the formula:


𝜎


FS


²


= 𝜎


scope


²


+ 𝜎


signal


²


𝜎


Att


²


= 𝜎


scope


²


+ ( 𝜎


signal


²


/ K ²


)
𝜎


scope


²


= (K ²


𝜎


Att


²


– 𝜎


FS


²


/ K ²


– 1)


where K is the attenuation value.


The 𝜎


scope


value is computed into the SNDRnr measurement results.


To use the Attenuator method, you will take a baseline measurement without attenuation, then connect a pair of 6 dB or 10 dB attenuators between the oscilloscope and DUT and measure again.


For the full step-by-step instructions, download our application note, *[Making New PCIe 6.0 Compliance Pattern Measurements with Your Oscilloscope](https://cdn.teledynelecroy.com/files/appnotes/pcie6compliance-measurements.pdf) .*


Reference waveforms must be saved on the same day you make measurements, using the exact same equipment. When you change any part of your setup, or start a new test session, choose Delete Reference and repeat the procedure to save it before proceeding with measurements.


#### Also see:


[New PCIe 6.0 Compliance Pattern Measurements](https://blog.teledynelecroy.com/2023/02/new-pcie-60-compliance-pattern.html)


[Get Ready for PCIe 6.0 Base Tx Testing--Compliance, Jitter and Eye Diagrams](https://blog.teledynelecroy.com/2022/06/get-ready-for-pci-express-60-base.html)
