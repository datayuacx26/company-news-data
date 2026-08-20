---
schema_version: "1.0.0"
document_id: "6f95e4c522ca76eb3fdc7daae93eab66d18ef9f5e16cd6fde351bc8e51b46bc7"
company_key: "yc-firezone"
company: "Firezone"
source_id: "yc-firezone-news-import-75fbee21a5d6"
canonical_url: "https://www.firezone.dev/blog/windows-1-5-13-mdm-update"
published_at: "2026-06-04T00:00:00+00:00"
first_seen_at: "2026-07-24T08:10:10.796715+00:00"
fetched_at: "2026-07-28T21:42:44.602232+00:00"
content_hash: "sha256:492daa89e69df96dc44c4e1c55217691890a26b59a4ba9430d240d8b759c9c35"
---

# Action required: Windows MDM policies move to the machine scope in 1.5.13

**Action required for MDM-managed fleets.** If you configure the Windows Client with Intune or another MDM, you must import the updated ADMX template and re-deploy your policy after upgrading to 1.5.13. Read on for the exact steps.


Starting with **Windows Client 1.5.13** , Firezone reads its MDM policy from the **machine-scoped** registry hive instead of the per-user hive. If you manage the Client centrally, you'll need to update how your policy is deployed.


## What changed


In 1.5.13 the Tunnel service — not the GUI app — now owns advanced settings and MDM configuration. The service is the privileged component that actually connects to the Firezone control plane, so it makes sense for it to be the one reading your managed settings. As a result, configuration now lives in a privileged, machine-wide location:


Before (≤ 1.5.12) After (1.5.13+)


Registry hive` HKEY_CURRENT_USER` (HKCU)` HKEY_LOCAL_MACHINE` (HKLM)


Full key` HKCU\\Software\\Policies\\Firezone`` HKLM\\Software\\Policies\\Firezone`


ADMX policy` class`` User`` Machine`


The subkey path (` Software\\Policies\\Firezone` ), the value names (` authURL` ,` apiURL` ,` accountSlug` ,` logFilter` ,` connectOnStart` ,` checkForUpdates` ,` hideAdminPortalMenuItem` ,` supportURL` ), and their types are all **unchanged** . Only the hive — and therefore the ADMX` class` — is different.


## Why you still have to act, even with auto-migration


The Client does include a one-time, best-effort migration: the first time the Tunnel service starts after the upgrade, it copies the connecting user's values from` HKCU\\Software\\Policies\\Firezone` into the machine hive and removes the old per-user key. It will not overwrite a machine-scoped policy you've already deployed.


That migration is only a grace period to keep existing installs working. It **does not** update your MDM configuration, and it runs at most once per machine. Your MDM continuously re-applies whatever you've configured — so if you leave the old (User-scoped) template in place, Intune will keep writing values into` HKCU` , where **1.5.13 no longer reads them** . To keep managing the fleet, you must re-import the updated ADMX and re-deploy your policy so values land in` HKLM` .


## Migrating in Microsoft Intune


1.


**Download the updated templates** from the Firezone repository:


- [firezone.admx](https://raw.githubusercontent.com/firezone/firezone/main/policy-templates/windows/firezone.admx)
- [firezone_en-US.adml](https://raw.githubusercontent.com/firezone/firezone/main/policy-templates/windows/firezone_en-US.adml)


2.


In the[Intune admin center](https://intune.microsoft.com/#view/Microsoft_Intune_DeviceSettings/DevicesMenu/~/configuration) , go to **Devices → Configuration** .


3.


**Delete the configuration profile** that referenced the old Firezone template, then **remove the previously imported Firezone ADMX** . Intune requires removing the old ADMX before importing a new version — see[Microsoft's documentation](https://learn.microsoft.com/en-us/intune/intune-service/configuration/administrative-templates-import-custom#replace-existing-admx-files) .


4.


Select **Import ADMX** and import the updated` .admx` and` .adml` files.


5.


Once the import succeeds, go to the **Policies** tab and choose **Create → New policy** . For **Platform** select **Windows 10 or later** , and for **Profile type** select **Templates → Imported Administrative templates** .


6.


Step through the wizard and re-enter your Firezone settings (` authURL` ,` accountSlug` , etc.), exactly as before.


7.


**Assign the profile to device groups.** Because the policy is now machine-scoped, assign it to the devices in your fleet rather than to user groups.


After the policy applies, users must **restart the Firezone Client** for the new configuration to take effect.


## Other deployment methods


- **Group Policy (AD / GPMC):** the Firezone settings now appear under **Computer Configuration → Administrative Templates** instead of **User Configuration** . Re-import the new ADMX into your Central Store and recreate the GPO objects accordingly.
- **Scripted / direct registry deployment:** change the target hive from` HKEY_CURRENT_USER\\Software\\Policies\\Firezone` to` HKEY_LOCAL_MACHINE\\Software\\Policies\\Firezone` . Writing to` HKLM` requires administrator privileges.


## Need a hand?


The full configuration reference, including every supported key, lives in our[Deploy the Clients](https://www.firezone.dev/kb/deploy/clients#distribute-clients-with-mdm) guide. If you run into trouble migrating your fleet,[reach out to support](https://www.firezone.dev/support) and we'll help you get sorted.
