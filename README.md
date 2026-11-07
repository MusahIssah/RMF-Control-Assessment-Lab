# RMF Control Assessment Lab: Windows Domain Controller (DC01)

A hands-on Risk Management Framework (RMF) control assessment that I ran as the assessor on a Windows Server 2022 domain controller in my home lab. The organization, Harbor Point Medical Group (HPMG), and every person, approval and policy in it are fictional. The technical evidence is real and comes from my own lab.

**Author:** Musah Issah  
**Period:** Nov 1 to Nov 7, 2026  
**Standards:** NIST SP 800-53 Rev 5.2.0 (Moderate baseline, SP 800-53B), assessment procedures from SP 800-53A, process from SP 800-37

[Download the assessment workbook](RMF_Control_Assessor_Workbook_Moderate_v2.xlsx) · [Browse the evidence](evidence/) · [Video link](https://youtu.be/nYuiG7V9qeQ)

## What this project shows

- I assess controls against 800-53A objectives and record Met, Not Met or Not Assessed for each one, with evidence.
- I protect evidence with SHA-256 hashes and log every file in an Evidence Log.
- I write findings with a risk level and a recommendation, track them as POA&Ms, and retest after the fix.
- I keep the as-found results unchanged and record every retest on its own tab.
- I report results honestly, including a change I could not explain and a fix that failed twice before it worked.

## The system and roles

| Item | Value |
| --- | --- |
| System | DC01, Windows Server 2022 domain controller, domain ad.northstar.test |
| Assessment type | Initial authorization |
| Baseline | NIST SP 800-53B Moderate |
| System owner (fictional) | Director of IT, HPMG |
| ISSO (fictional) | Information Security Officer (ISO) |
| Authorizing official (fictional) | Chief Executive Officer (CEO) |
| Assessor | Me |

The assessor tests the real system against what the organization claims and against the control requirements. The assessor reports findings. The system owner and ISSO own the SSP and the POA&M and send the authorization package to the authorizing official. My workbook has a POA&M tab so I can record findings and recommendations, but remediation belongs to the system owner.

## Method

1. **Scope the control.** Pull the 800-53A objectives for the control into the Objectives tab.
2. **Collect evidence.** Run read-only commands on DC01, save the output, and take a SHA-256 hash with `Get-FileHash`.
3. **Log it.** Add each file to the Evidence Log with its hash, source, date and the controls it supports.
4. **Examine, interview, test.** Review documents, interview the ISO, and test the system. Each objective gets a result.
5. **Record the finding.** Fill in the Control Assessment row (result, finding, risk, recommendation) and open a POA&M.
6. **Retest.** After remediation, collect new evidence and record the retest on the Retest Log. The Objectives and Control Assessment tabs keep the as-found results.

SHA-256 values refer to the original file bytes. Download the raw file before checking its hash.

## Results

I assessed 11 of the 287 controls and enhancements in the workbook. The others are Not Assessed. Ten were Other Than Satisfied at first, and one (AC-11(1)) was Satisfied.

### POA&Ms

7 closed, 1 ongoing, 2 open.

| ID | Control | Weakness | Risk | Status | Identified | Closed |
| --- | --- | --- | --- | --- | --- | --- |
| POAM-001 | AC-1 | Access control policy not disseminated | Low | Completed | 2026-11-01 | 2026-11-04 |
| POAM-002 | AC-2 | Account management gaps on DC01 | Moderate | Ongoing | 2026-11-01 |  |
| POAM-003 | AC-7 | Account lockout not enforced | High | Completed | 2026-11-02 | 2026-11-04 |
| POAM-004 | AC-8 | No system use notification | Low | Completed | 2026-11-02 | 2026-11-05 |
| POAM-005 | AC-11 | No inactivity lock on DC01 | Low | Completed | 2026-11-02 | 2026-11-05 |
| POAM-006 | AC-5 | No separation of duties on DC01 | Moderate | Open | 2026-11-02 |  |
| POAM-007 | AC-6 | Administrative privileges not limited on DC01 | High | Completed | 2026-11-02 | 2026-11-05 |
| POAM-008 | IA-5 | Weak password settings and no authenticator procedures | Moderate | Completed | 2026-11-02 | 2026-11-05 |
| POAM-009 | AU-2 | No audit policy and incomplete event logging | Moderate | Completed | 2026-11-03 | 2026-11-06 |
| POAM-010 | CM-6 | No configuration baseline or deviation process for DC01 | Moderate | Open | 2026-11-06 |  |

Highlights:

- **AC-7 lockout (POAM-003, High).** The domain lockout threshold was 0. A test account was not locked after 45 failed logons and DC01 logged no lockout. I set the threshold to 5 with a 15-minute duration and retested with a temporary test account, which locked after the fifth wrong password.
- **AC-6 privileges (POAM-007, High).** The built-in Administrator was the only enabled account and sat in Domain Admins, Enterprise Admins and Schema Admins. After approved requests, daily work moved to a standard account and admin work to a separate admin account, and the built-in account was disabled.
- **IA-5 authenticators (POAM-008).** Minimum password length was 7. I wrote an authenticator procedure and raised the minimum to 14.
- **AU-2 audit events (POAM-009).** No audit policy existed and failed logons were not logged. I wrote and approved a policy, applied the audit settings, raised the Security log from 128 MB to 1 GB, enabled archive-when-full, and held a coordination review. AU-02b was Not Met at interview and Met after remediation.
- **AC-2 and AC-5 (POAM-002, POAM-006).** Still open. The HR notification test for account removal (AC-02h.01) and separation of duties are not finished.

### Retests

Retest Log, 13 entries:

| Retest | POA&M | Control | Date | Result | Evidence |
| --- | --- | --- | --- | --- | --- |
| RT-001 | POAM-009 | AU-2 | 2026-11-03 | Partial | EV-0031, EV-0032 |
| RT-002 | POAM-001 | AC-1 | 2026-11-04 | Closed - Met | EV-0033, EV-0035 |
| RT-003 | POAM-003 | AC-7 | 2026-11-04 | Closed - Met | EV-0036, EV-0037 |
| RT-004 | POAM-007 | AC-6 | 2026-11-05 | Closed - Met | EV-0038, EV-0039, EV-0040 |
| RT-005 | POAM-004 | AC-8 | 2026-11-05 | Closed - Met | EV-0041, EV-0042 |
| RT-006 | POAM-005 | AC-11 | 2026-11-05 | Closed - Met | EV-0041, EV-0043 |
| RT-007 | POAM-008 | IA-5 | 2026-11-05 | Closed - Met | EV-0044, EV-0045 |
| RT-008 | POAM-002 | AC-2 | 2026-11-06 | Partial | EV-0038, EV-0040, EV-0046, EV-0047, EV-0048 |
| RT-009 | POAM-009 | AU-2 | 2026-11-06 | Partial | EV-0049, EV-0050 |
| RT-010 | POAM-009 | AU-2 | 2026-11-06 | Not Met | EV-0051 |
| RT-011 | POAM-009 | AU-2 | 2026-11-06 | Closed - Met | EV-0052, EV-0034 |
| RT-012 | POAM-009 | AU-2 | 2026-11-06 | Closed - Met | EV-0031, EV-0032, EV-0034, EV-0035, EV-0050, EV-0052, EV-0053 |
| RT-013 | POAM-010 | CM-6 | 2026-11-07 | Partial | EV-0057, EV-0063, EV-0059 to EV-0062, EV-0064, EV-0065, EV-0066 |

## Case study: CM-6, Configuration Settings

I took CM-6 through the whole cycle.

**As found (Nov 6).** DC01 had settings applied through Group Policy, but there was no documented baseline and no process to approve deviations. Audit and log settings were applied locally on the server instead of through Group Policy. Result: Other Than Satisfied, POAM-010 (Moderate).

**Remediation.**
- Wrote a fictional HPMG configuration baseline and deviation register (HPMG-STD-CM-001 v1.0) with 17 settings and a change control section. Two approved deviations are open until 2026-12-04: LDAP server signing set to negotiate instead of require (DEV-001), and interactive logon rights that include Account, Backup, Print and Server Operators (DEV-002). A third (DEV-003, audit and log settings applied locally) was closed by the change below.
- Wrote change ticket CHG-0001 to move the audit and log settings into the GPO, and got it approved before I made the change.

**The change failed twice.**
1. **Attempt 1 (EV-0060, failed).** The GPO applied the Security log settings, but it contained no audit settings and no audit extension, so every audit subcategory on DC01 fell back to No Auditing. Audit logging was off from about 20:21 to 20:48 Eastern, until I restored the policy with `auditpol` from the backup I took first (EV-0059).
2. **Attempt 2 (EV-0061, partial).** After registering the audit extension on the GPO, 16 audit settings applied. Credential Validation, Logoff, Directory Service Access and Network Policy Server were still No Auditing because the GPO did not set them. Failed logon auditing (event 4776) was off until the fix at 21:04 Eastern.
3. **Attempt 3 (EV-0062, passed).** The effective audit policy had all 20 subcategories on and matched the pre-change backup in 60 of 60 comparisons.

I kept all three results as evidence and recorded the lesson in the closed ticket (EV-0063): check the audit settings within five minutes of any change.

**An unexplained change.** The weekly GPO check (EV-0064) showed that the Default Domain Controllers Policy had been modified on 2026-11-02 with no ticket. I did not remember making the change. A Security log query for directory change events around that time found zero events (EV-0065), most likely because Directory Service Changes auditing was not on yet. I recorded it as unexplained, an incident and a deviation, with follow-up actions, in a weekly review record (EV-0066). I did not guess at a cause.

**Retest result (RT-013): Partial.** The baseline is documented and approved, most deviations are approved, and the weekly check works and caught a real change. CM-6 stays open until DEV-001 and DEV-002 are closed and the unexplained change is followed up.

## Evidence

66 items. Hash values are SHA-256. Files named with a number prefix came from my evidence collection script on Nov 1.

| ID | Title | Method | Collected | File | SHA-256 |
| --- | --- | --- | --- | --- | --- |
| EV-0001 | Access Control Policy and Procedures v1.0 | Examine | 2026-11-01 | [HPMG-POL-AC-001 v1.0.pdf](evidence/EV-0001_HPMG-POL-AC-001%20v1.0.pdf) | `11D333D1BB12F519E0485E019E18B060127717E8753DD7F43BCA35DEFDECA418` |
| EV-0002 | Evidence collection manifest (SHA-256 hashes) | Examine | 2026-11-01 | [00_Manifest.csv](evidence/EV-0002_00_Manifest.csv) | n/a (not hashed in log) |
| EV-0003 | System identity: DC01 OS and domain role | Examine | 2026-11-01 | [01_System_Identity.txt](evidence/EV-0003_01_System_Identity.txt) | `77B20B3B3DCA338340B461DFA83B22A4A60CC55AB563DE419EFA9DB9EFA1C571` |
| EV-0004 | Active Directory domain, forest and domain controllers | Examine | 2026-11-01 | [02_Domain_Forest.txt](evidence/EV-0004_02_Domain_Forest.txt) | `5220D6AA87B787C0E57AFC2E8AD843D10C0577FF07E77F79BEC2BE74A25F0B12` |
| EV-0005 | AD user account inventory | Examine | 2026-11-01 | [03_User_Accounts.txt](evidence/EV-0005_03_User_Accounts.txt) | `3BBC0595041B7EBB01CF7501626F766506843A7513BA34209C1C0509E2A9404B` |
| EV-0006 | Enabled accounts inactive 90 days or more | Examine | 2026-11-01 | [03b_Inactive_Accounts_90_Days.txt](evidence/EV-0006_03b_Inactive_Accounts_90_Days.txt) | `4AF9956D73DB84BABCE99F9FBD227F69AED880DA818090E8536A481664F5FB63` |
| EV-0007 | Privileged group membership | Examine | 2026-11-01 | [04_Privileged_Group_Members.txt](evidence/EV-0007_04_Privileged_Group_Members.txt) | `5C09912A50009B6EED2599E10C87F160DA79D0ADEC0C05F3E97C9B1F0B550FFC` |
| EV-0008 | Domain password and lockout policy | Examine | 2026-11-01 | [05_Password_Lockout_Policy.txt](evidence/EV-0008_05_Password_Lockout_Policy.txt) | `3307D88472767D6F155F91685DBD42EA266C3709DC54B6C9AB10361BF3CA76B8` |
| EV-0009 | Group Policy Object list | Examine | 2026-11-01 | [06_GPO_List.txt](evidence/EV-0009_06_GPO_List.txt) | `3A730994E3E68C3E95246683FDC8E1362972281B775114C72760AD083A64B891` |
| EV-0010 | Group Policy full settings report | Examine | 2026-11-01 | [06b_GPO_Report_All.html](evidence/EV-0010_06b_GPO_Report_All.html) | `5E8FCC4912BF5F7F339B7DB0C31E10E9D9B420FE96D96583B0D3C58E8957D37E` |
| EV-0011 | Advanced audit policy | Examine | 2026-11-01 | [07_Audit_Policy.txt](evidence/EV-0011_07_Audit_Policy.txt) | `0F14BA0E179F530D17B562E3D36162F1FB3B58F852DC42DBA28910F80836F4B5` |
| EV-0012 | Event log size and retention settings | Examine | 2026-11-01 | [08_Event_Log_Settings.txt](evidence/EV-0012_08_Event_Log_Settings.txt) | `23B71ECD8362C23615B75BCBAF605BDC6C36610FC206FC8759AE29AC0C902E85` |
| EV-0013 | Windows Time service configuration | Examine | 2026-11-01 | [09_Time_Configuration.txt](evidence/EV-0013_09_Time_Configuration.txt) | `0BF1D2FFFACAEDC4FD26AAC8CE5B674473CCCBB42D43D2CCFF032867FE3C4812` |
| EV-0014 | Session lock and logon banner settings | Examine | 2026-11-01 | [10_Session_Lock_and_Banner.txt](evidence/EV-0014_10_Session_Lock_and_Banner.txt) | `DF13801D827D3BAE5E5C5329394D108D03E160E9147953B296A934F5DB87BB16` |
| EV-0015 | Remote Desktop settings | Examine | 2026-11-01 | [11_Remote_Access_RDP.txt](evidence/EV-0015_11_Remote_Access_RDP.txt) | `277C0EFB829A7B81C38483F7960B305211778F90A7CCE17AA636BB6022D8F7D1` |
| EV-0016 | Windows Firewall profile settings | Examine | 2026-11-01 | [12_Firewall_Profiles.txt](evidence/EV-0016_12_Firewall_Profiles.txt) | `5493BFCC180C18A342704757D86BC812688FDBAD7175DAEFA17F7B7A6E5BC9A5` |
| EV-0017 | TCP listening ports and owning processes | Examine | 2026-11-01 | [12b_Listening_Ports.txt](evidence/EV-0017_12b_Listening_Ports.txt) | `DE23BA897D8E556E772D088F770013B28AA82B16FB4EAC45AD8D713F661CA331` |
| EV-0018 | SMB, LDAP, NTLM, TLS and FIPS settings | Examine | 2026-11-01 | [13_Protocol_and_Crypto_Settings.txt](evidence/EV-0018_13_Protocol_and_Crypto_Settings.txt) | `4A1F24F68A30D2F232E67CF8B0A979EE1609CAA3F5B3E955D0012016D5A1CBF0` |
| EV-0019 | Installed Windows roles and features | Examine | 2026-11-01 | [14_Installed_Roles_Features.txt](evidence/EV-0019_14_Installed_Roles_Features.txt) | `A369780F19D484A9687EEE3A93D849C267F8C0A335EF26699CA6E1F7F3B0085D` |
| EV-0020 | Running Windows services | Examine | 2026-11-01 | [14b_Running_Services.txt](evidence/EV-0020_14b_Running_Services.txt) | `787CDBD330AF8A178B430F1B1FB9E89FA1C9A25912724EA1689E5DAC12C28755` |
| EV-0021 | Installed Windows updates | Examine | 2026-11-01 | [15_Installed_Updates.txt](evidence/EV-0021_15_Installed_Updates.txt) | `21BC0B9A79FB62209D455E645BC8A025C95C88FE322ED18603C1AF698B1E27BF` |
| EV-0022 | Microsoft Defender status | Examine | 2026-11-01 | [16_Defender_Status.txt](evidence/EV-0022_16_Defender_Status.txt) | `901B49743A83900DC8F78AC0DEB4062E40F0E80EDD79124E45AED5FAFEE13B59` |
| EV-0023 | BitLocker status check (command failed) | Test | 2026-11-01 | [17_BitLocker_Status.txt](evidence/EV-0023_17_BitLocker_Status.txt) | `BA48469559D0F207C59457E43161E7CC30950AA56CCA9847C26438707E3164B1` |
| EV-0024 | Windows Server Backup check (command failed) | Test | 2026-11-01 | [17b_Backup_Versions.txt](evidence/EV-0024_17b_Backup_Versions.txt) | `4343A5CBB4760343D314F696BCB19E568D131BB34E98F51E085DC2D744975F43` |
| EV-0025 | Local security policy export | Examine | 2026-11-01 | [18_Local_Security_Policy.inf](evidence/EV-0025_18_Local_Security_Policy.inf) | `D87292199D47882D9819020C5CB137CA715C2610E7F23CB0582137CD440D97EE` |
| EV-0026 | AC-7 lockout test: jdoe not locked after repeated invalid logons | Test | 2026-11-02 | [EV-0026_AC-7_Lockout_Test_jdoe.png](evidence/EV-0026_EV-0026_AC-7_Lockout_Test_jdoe.png) | `3DC9F444D53B74A3B31D0F3FD779BCD87274A0D20BAEF93442F71E4676ED030B` |
| EV-0027 | Device lock screen on DC01: credentials required to resume | Test | 2026-11-02 | [EV-0027_AC-11_DC01_device_lock.png](evidence/EV-0027_EV-0027_AC-11_DC01_device_lock.png) | `B52836DCECD9ADE9F86A520710C96B0F04C6EC0479DB595E3961E2DBC982FC44` |
| EV-0028 | SVR02 Security log: failed logons for jdoe (event 4625) | Test | 2026-11-02 | [Assessment on SVR02; hashes match)](evidence/EV-0028_EV-0028_AC-7_SVR02_event4625.txt) | `7AAAACCB804EB07D53134DB1A0E33FBE3B312266AB25248E59417BC0C5F34475` |
| EV-0029 | DC01 Security log: no account lockout event (4740) for jdoe | Test | 2026-11-02 | [EV-0029_AC-7_DC01_no_lockout_event.png](evidence/EV-0029_EV-0029_AC-7_DC01_no_lockout_event.png) | `68147D857DA90D2E19632530DAC46E26F18A55AB70D0EBA3E8629181FE8A7DC0` |
| EV-0030 | No audit and accountability policy or procedures exist for DC01 | Examine | 2026-11-03 | (absence of a document; no file) | n/a |
| EV-0031 | DC01 advanced audit policy after remediation (retest) | Examine | 2026-11-03 | [AuditPolicy_after.txt](evidence/EV-0031_AuditPolicy_after.txt) | `FC764872FB6199AC356019B798543F9EA8598DD6D0DBF6A9A27CAE8EA92C9E97` |
| EV-0032 | Failed logon test on DC01 after remediation (retest) | Test | 2026-11-03 | [FailedLogon_events.txt](evidence/EV-0032_FailedLogon_events.txt) | `EE6B9FCF251BB6AAAC84D68BA0C66B57E43878715B355A41744ECB2BC5452397` |
| EV-0033 | Access Control policy v1.0, approved (retest) | Examine | 2026-11-04 | [Access Control Policy and Procedures (HPMG, practice document).pdf](evidence/EV-0033_Access%20Control%20Policy%20and%20Procedures%20%28HPMG%2C%20practice%20document%29.pdf) | `BC11E0BCE4E8EBAC9EEA92A3C10EDDA2C2AA854ABC7EAD0E6BFBC5C9F90BDF1F` |
| EV-0034 | Audit and Accountability policy v1.0, approved (remediation) | Examine | 2026-11-04 | [Audit and Accountability Policy and Procedures (HPMG, practice document).pdf](evidence/EV-0034_Audit%20and%20Accountability%20Policy%20and%20Procedures%20%28HPMG%2C%20practice%20document%29.pdf) | `7944BCC2D36A8B32D57C6E8D25CD0C22593FD491671DB759224B4BF4229C7F96` |
| EV-0035 | Policy dissemination and acknowledgment record | Examine | 2026-11-04 | [Policy Dissemination and Acknowledgment Record (HPMG, practice document).pdf](evidence/EV-0035_Policy%20Dissemination%20and%20Acknowledgment%20Record%20%28HPMG%2C%20practice%20document%29.pdf) | `5964F3DE8D10FE9BB0BBB4FB8FBCCCD6A5EBE7E45E1D9B399B97FEB040346BBD` |
| EV-0036 | Domain lockout policy after remediation (retest) | Examine | 2026-11-04 | [LockoutPolicy_after.txt](evidence/EV-0036_LockoutPolicy_after.txt) | `E874EE22D22FA3D4BC6CFC0219D87BA418BB33C0F08EBD298EBDCB40F9A5DEF3` |
| EV-0037 | Account lockout test on DC01 after remediation (retest) | Test | 2026-11-04 | [LockoutTest_DC01.txt](evidence/EV-0037_LockoutTest_DC01.txt) | `1579FF8A8571AE70BCDE3B335C4C9E2EE5FA39B65FBA75F787A2933495B66778` |
| EV-0038 | Approved access requests REQ-0001 and REQ-0002 | Examine | 2026-11-04 | [Access Requests and Account Register (HPMG, practice document).pdf](evidence/EV-0038_Access%20Requests%20and%20Account%20Register%20%28HPMG%2C%20practice%20document%29.pdf) | `68072DAC250FBD7041989A0426BEFE92C9264C53671D8F4F22B9C93357CA098C` |
| EV-0039 | Privileged group membership after remediation (retest) | Examine | 2026-11-04 | [PrivilegedGroups_after.txt](evidence/EV-0039_PrivilegedGroups_after.txt) | `E155F1F47E7607A075AE546296E8522FA4E38E7A659FA5393E2EF3B549B030F6` |
| EV-0040 | AD account inventory after remediation (retest) | Examine | 2026-11-04 | [AccountInventory_after.txt](evidence/EV-0040_AccountInventory_after.txt) | `2696243A17CD91644AF5CADDBF23202C675BD0753837FD83780CC69AC67407BC` |
| EV-0041 | Effective inactivity limit and logon banner on DC01 (retest) | Examine | 2026-11-05 | [BannerLock_after.txt](evidence/EV-0041_BannerLock_after.txt) | `55698E5F121E2AB49D5BE8100A7B6EF580B285B61AEDBC27066D019037EE60C2` |
| EV-0042 | DC01 logon banner screenshot (retest) | Test | 2026-11-05 | [AC8_banner_DC01.png](evidence/EV-0042_AC8_banner_DC01.png) | `0C4153FC4F3977228B3EB149A5B314FD9547A39454544B911875B3AB46968176` |
| EV-0043 | DC01 inactivity lock screenshot (retest) | Test | 2026-11-05 | [AC11_inactivity_lock_DC01.png](evidence/EV-0043_AC11_inactivity_lock_DC01.png) | `452F468C0373B3694B57A30FF291C3F19E9AEAFA5C445E2C8FF87C2DD8F64489` |
| EV-0044 | Authenticator Management Procedure v1.0, approved (remediation) | Examine | 2026-11-05 | [HPMG-PRO-IA-001_v1.0_approved.pdf](evidence/EV-0044_HPMG-PRO-IA-001_v1.0_approved.pdf) | `61D571E30625FD8190A461308CDECB56BE961B1897455458B4595C948057F7B6` |
| EV-0045 | Domain password policy after remediation (retest) | Examine | 2026-11-05 | [PasswordPolicy_after.txt](evidence/EV-0045_PasswordPolicy_after.txt) | `8E1BC20FFAA4D3F0E2E3364660EE17061771C2355C8D438585799BC71F053C70` |
| EV-0046 | Account types, register and first access review (remediation) | Examine | 2026-11-05 | [HPMG_Account_Register_Review_20261105.pdf](evidence/EV-0046_HPMG_Account_Register_Review_20261105.pdf) | `94E2CD039A68DB829CB038BB38A7B3A741C5D3ACB6829E5F2EBF11F7CDBD4EEB` |
| EV-0047 | DC01 weekly review: account management events, 7 days | Examine | 2026-11-06 | [WeeklyReview_events_v2.txt](evidence/EV-0047_WeeklyReview_events_v2.txt) | `9B2D3CA0FE762F6BEB5BA5EF9B9717E5DB5349600D7834F10AEC46810988A8B8` |
| EV-0048 | Weekly log review record, 2026-11-06 | Examine | 2026-11-06 | [HPMG_Weekly_Review_20261106.pdf](evidence/EV-0048_HPMG_Weekly_Review_20261106.pdf) | `E3BA9C1581D1D5358D4E7CE6833416047B3C062AB4064B656C53B3FD107F928F` |
| EV-0049 | DC01 Security log settings before change (baseline) | Examine | 2026-11-06 | [SecurityLog_before.txt](evidence/EV-0049_SecurityLog_before.txt) | `B303B2BC7A7AB4AC30632C8EB1F874D87C04CAEE6C614D761D84613F5E553B9E` |
| EV-0050 | DC01 Security log settings after change (retest) | Examine | 2026-11-06 | [SecurityLog_after.txt](evidence/EV-0050_SecurityLog_after.txt) | `45A707B6A3175F3645031D73E313CB675E5B2D7CA74486D1E0A128A11D8529B3` |
| EV-0051 | Interview notes, AU-02b coordination of event logging (retest) | Interview | 2026-11-06 | [HPMG_Interview_AU-02b.pdf](evidence/EV-0051_HPMG_Interview_AU-02b.pdf) | `109F18385C124F2DC532D18BB98C4831DE54F577EC2DE6026EADB92A06FBE62E` |
| EV-0052 | Audit event coordination record HPMG-REC-AU-001 (remediation) | Interview | 2026-11-06 | [HPMG_Coordination_Record_AU](evidence/EV-0052_HPMG_Coordination_Record_AU.pdf) | `41A7D5A5808833BA3680F61BA946D23A4D39AC5289DBEF69E90194F7860FD106` |
| EV-0053 | DC01 Security log archive-when-full setting after change (retest) | Examine | 2026-11-06 | [SecurityLog_archive_after.txt](evidence/EV-0053_SecurityLog_archive_after.txt) | `150BAE65F04143277753B285DD07FAFBDDFFC23E3A0D3EB08DC2B81F084B91C7` |
| EV-0054 | DC01 Group Policy list and applied computer settings (as found, CM-6) | Examine | 2026-11-06 | [ConfigSettings_asfound.txt](evidence/EV-0054_ConfigSettings_asfound.txt) | `1364673C489F34524513B98E803E8206AFCB73F6F79E035CCF4866853560C9E9` |
| EV-0055 | DC01 effective audit policy, LAB - DC01 Security Hardening GPO report and Domain Controllers OU links (as found, CM-6) | Examine | 2026-11-06 | [ConfigCheck_asfound.txt](evidence/EV-0055_ConfigCheck_asfound.txt) | `7429C54613265B5855BAAC775C0FB466D606C2C025A19DA000DAC326335CC5B3` |
| EV-0056 | Interview notes, CM-6 configuration settings (as found) | Examine | 2026-11-06 | [HPMG_Interview_CM-6.pdf](evidence/EV-0056_HPMG_Interview_CM-6.pdf) | `E54EA72A6588802AADF617D81D3C5B65D94F7C3857DD0FEF165CC72D408CE93D` |
| EV-0057 | DC configuration baseline and deviation register HPMG-STD-CM-001 v1.0, approved (remediation) | Examine | 2026-11-06 | [HPMG_Config_Baseline.pdf](evidence/EV-0057_HPMG_Config_Baseline.pdf) | `A1F427EE7A1FC2AD395D160801D02AB9A891FBC09C7CF8CC54891E5B3DD7F548` |
| EV-0058 | Change ticket CHG-0001, approved before implementation (remediation) | Examine | 2026-11-06 | [CHG-0001_approved.pdf](evidence/EV-0058_CHG-0001_approved.pdf) | `48F2CD351CEC0606B02FEC1220666AF90061EBAE1B78A985329210B791182B76` |
| EV-0059 | DC01 audit policy backup before CHG-0001 (rollback point) | Examine | 2026-11-06 | [AuditPolicy_before_CHG0001.csv](evidence/EV-0059_AuditPolicy_before_CHG0001.csv) | `57540D91309F6F2955F80C2470AC4BE4094B03A76782A4164FD7F236961EA8C5` |
| EV-0060 | CHG-0001 verification, attempt 1 (failed, rolled back) | Examine | 2026-11-06 | [CHG0001_after.txt](evidence/EV-0060_CHG0001_after.txt) | `234605BDC76961164FCD5C34C8FA1BC2155431B664B84636B7669ACA393E28D8` |
| EV-0061 | CHG-0001 verification, attempt 2 (partial) | Examine | 2026-11-06 | [CHG0001_after2.txt](evidence/EV-0061_CHG0001_after2.txt) | `538E93D91CE5F95C4A4D7F8BEC2655E53AC10EAFCD74613FC91C4947C909B4C5` |
| EV-0062 | CHG-0001 verification, attempt 3 (passed) | Examine | 2026-11-06 | [CHG0001_after3.txt](evidence/EV-0062_CHG0001_after3.txt) | `405B22C6AF9EE123A0CBF0E5E44C7AEBA4FAFF91907D4F279DAE006633003F7C` |
| EV-0063 | Change ticket CHG-0001, closed with results of three attempts (remediation) | Examine | 2026-11-06 | [CHG-0001_closed.pdf](evidence/EV-0063_CHG-0001_closed.pdf) | `68D78F9BF165EC8B43511A38F5BE7E816EF3CC353FB3D9F45A93FD592588FD24` |
| EV-0064 | DC GPO modification list, weekly check of 2026-11-06 | Examine | 2026-11-06 | [WeeklyGPOCheck_2026-11-06.txt](evidence/EV-0064_WeeklyGPOCheck_2026-11-06.txt) | `EBDFC48140487125337776861EDB0E0F360B7D0423CC7DA8C480787DB41E82FE` |
| EV-0065 | Security Log Query – Unexplained Default Domain Controllers Policy Change (2026-11-02) | Examine | 2026-11-07 | [DDCP_investigation_2026-11-06.txt](evidence/EV-0065_DDCP_investigation_2026-11-06.txt) | `509AB4C2A3BCAC46932A8834EB4B2422EEA5CF9E100A6650BCB379B1FA851F07` |
| EV-0066 | Weekly GPO Change Review 2026-11-06 (HPMG-REC-CM-002) | Examine | 2026-11-07 | [HPMG_Weekly_GPO_Review_2026-11-06.pdf](evidence/EV-0066_HPMG_Weekly_GPO_Review_2026-11-06.pdf) | `A7E040B15F553BC334EF8C4F1A8B160B0D577EC58D733B759324169E082B88B5` |

## Limitations

- **Partial scope.** 11 controls of 287 assessed. There is no SSP. I assessed the system against the organization's policies and the control text.
- **One person.** I played the assessor and also did the remediation in a system administrator role, so retests are not fully independent. The Retest Log records this.
- **Fictional organization.** HPMG, its policies and approvals are made up for practice. The ISO's interview answers, the coordination record and approvals are written by me and are labeled fictional in the documents.
- **Lab clock.** DC01's clock is Pacific time, about 3 hours behind Eastern. Evidence notes record times in DC01 time where it matters.
- **Open items.** POAM-002, POAM-006 and POAM-010 are not closed. The policy needs a v1.1 (group removal events 4729, 4733 and 4757, and the deviation updates), and baseline v1.1 will add the unexplained change as DEV-004.

## Files

- `README.md`: this summary.
- `RMF_Control_Assessor_Workbook_Moderate_v2.xlsx`: the assessor workbook (Summary, Control Assessment, Objectives, Evidence Log, POAM Findings, Retest Log).

I used Claude (Anthropic) as a coach and drafting tool: it walked me through each step, drafted the fictional HPMG documents, and checked my hashes. I ran every command on my own lab and entered every result myself.
