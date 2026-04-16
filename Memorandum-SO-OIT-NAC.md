# Memorandum

DATE: April 14, 2026

TO: Office of Information Technology

FROM: David Geeraerts, Scientific Computing

SUBJECT: Network Access Control (802.1X Authentication) Using Active Directory


## Summary

In order for Scientific Computing to participate with Network Access Control (802.1X Authentication), Scientific Computing proposes a configuration update to the existing ClearPass implementation to enable Active Directory (AD) Authentication and Authorization.
To achieve a viable and scalable NAC solution, Scientific Computing needs to transition away from the current KACE-dependent implementation in favor of a native AD-based approach.


## The Problem: KACE Integration Limitations

- **Manual Overhead:** KACE Asset record requires manual data entry for the "Device Function" field, as the KACE agent only creates an inventory record that then has to be linked to the asset record in KACE. This is problematic when KACE contains more than 1 asset record for a computer. The fact that this front end configuration is required in KACE, it breaks zero-touch deployment for client computers, which introduces operational friction for deployment. 
- **Inadequate Security:** Relying on KACE attributes tied to a computer's MAC address does not constitute real authentication —it merely associates a hardware identifier without verifying identity. Active Directory, in contrast, enables true authentication for computers and users through domain credentials and certificates, aligning with best practice security standards.
- **Trust and Integrity Violation:** The KACE agent functions as persistent software with elevated privileges, akin to malware that could be exploited. This violates the principle of least privilege and introduces an unnecessary attack vector.


## The Solution: AD-Based Authentication & Authorization

Integrating Active Directory as the authentication & authorization source for ClearPass provides a secure, automated alternative that addresses KACE's shortcomings:

- **Automated Role Mapping:** Using the Distinguished Name (DN) or OU path of computer objects, ClearPass can dynamically assign network roles without manual intervention, enabling zero-touch deployment.
- **Real-Time Accuracy:** Permissions update instantly when devices are joined, moved, or reconfigured in AD, ensuring alignment with live domain state and eliminating stale data.
- **Enhanced Security:** AD enables true authentication for computers and users through domain credentials and certificates, providing robust identity verification that MAC-based methods cannot match.


## Conclusion

Leveraging an existing Inventory Management System (KACE) into Network Access Control for 802.1X authentication, though convenient, has security limitations and is thus not the best technical solution, nor the industry best practice. The path forward for Scientific Computing is an Active Directory implementation for NAC. While on-premises AD is preferred for NAC, if Microsoft Intune integration is required for broader endpoint management, Scientific Computing is eager to collaborate on Intune integration.

