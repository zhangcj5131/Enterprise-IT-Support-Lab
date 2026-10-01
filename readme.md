![architecture](./readme.assets/architecture.png)

# Northstar Technologies — IT Support Lab Implementation Roadmap

## Project Goal

Build a realistic small-business Windows IT environment for hands-on **Help Desk / IT Support** practice.

The completed lab will integrate:

- Windows Server 2022
- Active Directory Domain Services (AD DS)
- DNS
- DHCP
- Group Policy
- File Server and NTFS/Share permissions
- Windows 11 domain-joined workstation
- Microsoft Entra ID
- Microsoft Entra Connect / Hybrid Identity
- Microsoft 365
- Microsoft Intune
- Windows 11 Entra-joined and cloud-managed workstation
- Common IT support and troubleshooting scenarios

The purpose of the project is not to build a large enterprise infrastructure. The goal is to create a compact environment that demonstrates the technologies and troubleshooting skills commonly required in **Help Desk / IT Support / Junior System Administration** roles.

---

# 1. Hyper-V Lab Environment

## Objective

Build the virtualization and networking foundation for the entire project.

## Environment

Three virtual machines are used:

| Machine  | Operating System    | Primary Purpose              |
| -------- | ------------------- | ---------------------------- |
| DC01     | Windows Server 2022 | Server infrastructure        |
| CLIENT01 | Windows 11 Pro      | On-premises domain client    |
| CLIENT02 | Windows 11 Pro      | Cloud-managed / Entra client |

Network:

```text
Subnet:          192.168.88.0/24
Gateway:         192.168.88.1

DC01:            192.168.88.12
CLIENT01:        192.168.88.13
CLIENT02:        192.168.88.14
```

All machines are connected through a Hyper-V External Virtual Switch.

DC01 uses a stable static IP and fixed virtual MAC address because it will provide core network and identity services.

CLIENT01 and CLIENT02 also use predictable addresses to simplify lab administration and troubleshooting.

## Completion Goal

At the end of this chapter:

- DC01 is operational.
- CLIENT01 is operational.
- CLIENT02 is operational.
- All machines have network connectivity.
- Internet connectivity works.
- Remote administration works where required.

**Current status: COMPLETED**

---

# 2. Active Directory Domain Services and DNS

## Objective

Build the core on-premises Windows domain environment.

DC01 will become the Domain Controller and DNS server for the lab.

## Planned Environment

```text
Domain: corp.lab

DC01
├── Active Directory Domain Services
└── DNS Server
```

## Active Directory Structure

Create a realistic organizational structure for a small company.

Example:

```text
corp.lab
│
├── Users
│   ├── IT
│   ├── HR
│   ├── Finance
│   └── General
│
├── Computers
│   ├── Workstations
│   └── Servers
│
└── Groups
    ├── IT Groups
    ├── HR Groups
    ├── Finance Groups
    └── General Groups
```

Create representative:

- Organizational Units (OUs)
- User accounts
- Security groups
- Administrative accounts
- Computer objects

Practice basic Active Directory administration including:

- Creating and disabling users
- Resetting passwords
- Unlocking accounts
- Managing group membership
- Moving objects between OUs
- Understanding authentication and authorization

## DNS

Configure and verify AD-integrated DNS.

Practice:

- Forward lookup
- A records
- CNAME records
- AD-related DNS records
- Internal hostname resolution
- Basic DNS troubleshooting

## CLIENT01 Domain Join

CLIENT01 will be configured to use DC01 for DNS and then joined to:

```text
corp.lab
```

CLIENT01 becomes the primary traditional enterprise workstation.

Verify:

- Domain Join
- Domain user login
- Domain authentication
- DNS resolution
- Communication with DC01

## Completion Goal

At the end of this chapter:

```text
DC01
   ↓
Active Directory + DNS
   ↓
corp.lab
   ↓
CLIENT01
Domain Joined
```

A functioning Windows domain environment exists.

---

# 3. DHCP and Windows Network Services

## Objective

Add centralized IP address management and practice common Windows network administration tasks.

## DHCP Server

Install and configure DHCP on DC01.

Create a DHCP scope for the lab network.

Practice:

- DHCP Scope
- Address Pool
- Exclusion Range
- Lease
- Reservation
- DHCP Options
- Default Gateway
- DNS Server assignment

Use CLIENT01 to verify DHCP operation and practice common DHCP troubleshooting scenarios.

## Important Design Note

DC01 remains statically addressed.

Infrastructure servers should not depend on dynamically assigned addresses.

## Completion Goal

DC01 provides centralized:

```text
AD DS
DNS
DHCP
```

The lab now contains the basic infrastructure services commonly found in a Windows business network.

---

# 4. Group Policy and File Server

## Objective

Add centralized workstation management and company file-sharing services.

This chapter turns the basic domain into a more realistic business environment.

---

## 4.1 Group Policy

Create several practical Group Policy Objects (GPOs).

Possible policies include:

- Password/security settings
- Desktop or workstation configuration
- Windows settings
- Drive mapping
- Basic security restrictions
- Windows Update-related configuration

Practice:

- Creating GPOs
- Linking GPOs to OUs
- User vs. Computer Configuration
- Policy inheritance
- `gpupdate`
- `gpresult`
- Troubleshooting policies that do not apply

CLIENT01 will be the primary machine used to verify Group Policy behavior.

---

## 4.2 File Server

Configure shared company folders on DC01.

Example:

```text
\\DC01\Shares

├── IT
├── HR
├── Finance
└── Public
```

Create department-based access using Active Directory security groups.

Practice:

- NTFS permissions
- Share permissions
- Group-based access
- Least privilege
- AGDLP-style permission management
- Access troubleshooting

Users from different departments should receive different access rights.

## Completion Goal

The on-premises environment now provides:

```text
DC01
├── Active Directory
├── DNS
├── DHCP
├── Group Policy
└── File Server

        ↓

CLIENT01
└── Domain-managed workstation
```

At this stage, the traditional on-premises Windows environment is essentially complete.

---

# 5. Microsoft Entra ID and Microsoft 365

## Objective

Extend the on-premises identity environment into Microsoft's cloud services.

This chapter introduces modern cloud identity and Microsoft 365 administration.

---

## 5.1 Microsoft Entra ID

Create and configure the Microsoft Entra ID environment.

Practice:

- Users
- Groups
- Roles
- MFA
- Licenses
- Basic identity administration

---

## 5.2 Microsoft 365

Connect users to Microsoft 365 services.

Practice basic administration involving:

- Microsoft 365 user accounts
- Licensing
- Outlook / Exchange Online
- Teams
- OneDrive
- Basic SharePoint access

The focus is Help Desk-level administration rather than advanced Microsoft 365 engineering.

---

## 5.3 Microsoft Entra Connect / Identity Synchronization

Connect the on-premises Active Directory environment with Microsoft Entra ID.

Target architecture:

```text
On-Premises AD
     │
     │ Microsoft Entra Connect
     ↓
Microsoft Entra ID
     │
     ↓
Microsoft 365
```

Synchronize selected users and groups from:

```text
corp.lab
```

to Microsoft Entra ID.

Practice understanding and troubleshooting:

- On-premises users
- Cloud-only users
- Synced users
- Synced groups
- Identity synchronization
- Licensing after synchronization

## Completion Goal

The project now contains a hybrid identity environment connecting traditional Active Directory with Microsoft's cloud identity platform.

---

# 6. Microsoft Intune and CLIENT02

## Objective

Build a modern cloud-managed Windows endpoint.

CLIENT02 will represent a workstation that is managed primarily through Microsoft cloud services rather than the traditional on-premises domain.

## CLIENT02 Design

CLIENT02 will be:

```text
Windows 11 Pro
        ↓
Microsoft Entra Joined
        ↓
Microsoft Intune Enrolled
        ↓
Cloud Managed
```

CLIENT02 will NOT be used as the primary traditional `corp.lab` domain workstation.

CLIENT01 and CLIENT02 therefore represent two different endpoint-management models:

```text
CLIENT01
On-Premises Domain Joined
AD + GPO

CLIENT02
Entra Joined
Intune Managed
```

## Intune Tasks

Practice:

- Device enrollment
- Device inventory
- Configuration profiles
- Compliance policies
- Basic security policies
- Application deployment
- Remote management actions

Verify that CLIENT02 receives policies and applications from Intune.

## Completion Goal

The lab now demonstrates both:

**Traditional Windows management**

```text
AD → GPO → CLIENT01
```

and:

**Modern cloud endpoint management**

```text
Entra ID → Intune → CLIENT02
```

---

# 7. IT Support and Troubleshooting Scenarios

## Objective

Use the completed infrastructure to simulate realistic Help Desk tickets.

This chapter converts the infrastructure project into practical IT Support experience.

The emphasis is not simply configuring technology, but diagnosing and resolving user problems.

---

## 7.1 User Lifecycle

Simulate employee onboarding.

Example workflow:

```text
New Employee
      ↓
Create AD Account
      ↓
Assign Department / OU
      ↓
Add Security Groups
      ↓
Provide File Access
      ↓
Microsoft 365 License
      ↓
Verify Login and Services
```

Also simulate employee offboarding:

- Disable account
- Remove group access
- Revoke access
- Remove or adjust licensing
- Document account status

---

## 7.2 Active Directory Support Cases

Practice common tickets such as:

- User cannot log into the domain
- Forgotten password
- Locked account
- Incorrect group membership
- User cannot access a company resource

---

## 7.3 DNS and Network Support Cases

Practice:

- Client cannot resolve internal hostname
- Incorrect DNS configuration
- DHCP/address problems
- Connectivity troubleshooting
- `ipconfig`
- `ping`
- `nslookup`

---

## 7.4 Group Policy Support Cases

Simulate:

- GPO does not apply
- User receives incorrect policy
- Computer is placed in the wrong OU
- Policy has not refreshed

Use tools such as:

```text
gpupdate
gpresult
```

to diagnose the problem.

---

## 7.5 File Permission Support Cases

Simulate:

- User cannot access department share
- Incorrect security group membership
- NTFS permission problem
- Share permission problem

Determine whether the failure occurs at:

```text
User
  ↓
Group Membership
  ↓
Share Permission
  ↓
NTFS Permission
  ↓
Resource
```

---

## 7.6 Microsoft 365 / Entra Support Cases

Practice:

- User cannot access Microsoft 365
- Missing license
- MFA issue
- Group membership not synchronized
- Synced user problem
- Cloud account vs. on-premises account troubleshooting

---

## 7.7 Intune Support Cases

Practice:

- Device does not enroll
- Configuration profile does not apply
- Compliance policy does not apply
- Application deployment fails
- Device does not appear correctly in Intune

---

# Final Architecture

When all seven chapters are complete, the lab should represent the following environment:

```text
                     NORTHSTAR TECHNOLOGIES
                              │
          ┌───────────────────┴────────────────────┐
          │                                        │
   ON-PREMISES                                MICROSOFT CLOUD
          │                                        │
        DC01                                Microsoft Entra ID
 Windows Server 2022                               │
          │                                  Microsoft 365
          ├── Active Directory                     │
          ├── DNS                                  │
          ├── DHCP                               Intune
          ├── Group Policy                         │
          └── File Server                          │
          │                                        │
          │                                        │
      CLIENT01                                 CLIENT02
 Windows 11 Pro                              Windows 11 Pro
 Domain Joined                               Entra Joined
 GPO Managed                                 Intune Managed
          │                                        │
          └──────── Microsoft Entra Connect ───────┘
                    Identity Synchronization
```

---

# Project Completion Criteria

The project is complete when I can independently demonstrate and explain:

1. How a Windows domain is built and administered.
2. How users, groups, computers, and OUs are managed in Active Directory.
3. How DNS and DHCP support a Windows enterprise environment.
4. How Group Policy centrally manages Windows clients.
5. How NTFS and Share permissions control company file access.
6. How an on-premises Windows client joins and operates in a domain.
7. How Active Directory identities can integrate with Microsoft Entra ID.
8. How Microsoft 365 users, licenses, MFA, and basic services are administered.
9. How Windows devices can be Entra joined and managed through Intune.
10. How to diagnose and resolve common Help Desk problems across these systems.

The final goal is not simply to say that these technologies were studied.

The final goal is to be able to say:

> **I built the environment, configured the services, connected the clients, intentionally created common support problems, diagnosed them, and fixed them.**