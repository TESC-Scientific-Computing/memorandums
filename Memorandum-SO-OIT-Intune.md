# Memorandum

TO: Office of Information Technology

FROM: Science Operations

PREPARED BY: David Geeraerts

DATE: April 17, 2026

SUBJECT: Scientific Computing Client Management Transition: Microsoft Entra ID (Hybrid Join) & Microsoft Intune.


## I. Statement
The migration to Entra ID (Hybrid Join) & Microsoft Intune represents a fundamental shift from on-premises Active Directory toward a cloud endpoint management framework. The Sciences recognize the potential security and operational benefits of a hybrid environment that bridges on-premises infrastructure with Microsoft’s cloud-native management tools.

However, the specialized nature of scientific computing —including the management of complex instrumentation, specialized lab software, and a dynamic curriculum— requires a management model that preserves technical and administrative agility. To ensure that this transition enhances rather than hinders our academic and research mission, a distributed and delegated responsibility model is essential.
Therefore, Science (via Scientific Computing) participation in Microsoft Entra Hybrid Join & Microsoft Intune is contingent on scientific computing maintaining distributed and delegated administration over the systems that affect our computing infrastructure.

## II. Technical Discovery

- Confirmation : Tenant ID = `00000000-0000-0000-0000-000000000000`

- SCP (Service Connection Point) key does not include the expected Entra ID device registration GUID typically required for Hybrid Join in AD. SCP verification [`CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=evergreen,DC=edu`] is missing fixed GUID `62a0ff2e-d1b5-4a69-a3b0-0b619943324d`

- Is the tenant using Intune Scope Tags to maintain delegated management for devices? 

- Is the ControlPolicyConflict/MDMWinsOverGP CSP currently set at the tenant level? 

- What is Entra Connect setting for syncs? (Default is 30 minutes) 

- Are there existing "All Devices" Configuration Profiles or Security Baselines that will overlap with our delegated OU? 

- Is OIT piloting or planning deployment of Security Copilot agents {Change Review Agent, Device Offboarding Agent, Policy Configuration Agent, Vulnerability Remediation Agent}? 

- What tool {KACE, Teams, Email} to use as an audit log for documenting obstacles and monitoring progress for the project?


## III. Technical Framework & Delegation Model

1. Active Directory [`DC=evergreen,DC=edu`]
   - Rename SciComp to ScientificComputing [`OU=ScientificComputing,OU=Workstations,DC=evergreen,DC=edu`]
   - Delegated root administration {READ, WRITE, DELETE} security permissions for `OU=ScientificComputing,OU=Workstations,DC=evergreen,DC=edu` 

2. Microsoft Intune admin center
(_Achieved using a combination of custom Intune RBAC role, a dedicated Scope Tag (“Scientific-Computing”), and appropriate Entra ID group ownership.)_
 - Entra ID custom role for Scientific-Computing
   - manage "Devices"
   - Device Group Management {dynamic, static}
   - manage Groups
   - manage MDM Configuration Profiles
   - manage Endpoint Security
   - manage Mobile Apps
   - access to WCD (Windows Configuration Designer) for Provisioning Packages to support zero-touch automation
 - Scope Tags for Scientific-Computing
 - Read-Only access to all MDM Configuration Profiles that touch Scientific-Computing 

## IV. Phased Implementation Strategy

### Setup Phase
- Active Directory setup for `OU=ScientificComputing,OU=Workstations,DC=evergreen,DC=edu`
- Intune portal access
- Set up Microsoft Entra ID roles and licensing
- Microsoft Defender for Endpoint portal access

### Testing Phase
- Intune access verification
- Validate Imaging workflow
- Verification `dsregcmd /join` for zero-touch automation Provisioning Packages (using WCD).
- Verification: `dsregcmd /status`
- Entra ID Groups
- MDM Configuration Profiles
- Intune Scope Tags
- Service Connection Point (SCP)
- Test Entra ID Groups using the device.deviceManagementAppId
- Microsoft Defender for Endpoint portal access
- Verification User Shared Computer Licensing for Office 365
- Verification of no local user or group changes -- important for Scientific Instrumentation, and SSC laptops.

### Pilot Phase
- Non-critical auxiliary PC's

### First Migration Phase
- CAL (Computer Applications Lab)
- SSC (Science Support Center) Operations
- SSC Laptop pool

### Secondary Migration Phase
- Scientific Instrumentation
- verification of strict constraint on local user or group changes
 
 
