# Active Directory Security Lab
Automated Active Directory lab featuring GPO security baselines, Windows LAPS, advanced audit policies, and PowerShell identity provisioning.

# Tools
- Hypervisor - VirtualBox
- Server - Windows Server 2012 R2
- Client - Windows Server 2012 R2
- Security telemetry and threat monitoring - Windows Event Viewer, Windows Advanced Audit Policies and Process Command-Line Logging

# Network Diagram

# Installation and Configuration
The configuration of Active Directory lab in practice desires one server machine - that will serve as domain controller - and the other one - client machine to test the work of domain. As mentioned earlier, we use VirtualBox as a hypervisor together with Windows Server 2012 R2 installations (server is promoted to domain controller, while the client is not). Due to environment baseline constraints, security telemetry and threat monitoring were implemented using native Windows Advanced Audit Policies and Process Command-Line Logging (Event ID 4688) rather than third-party agents, demonstrating enterprise audit compliance and native Event Log analysis.

Domain controller needs to be configured in order to run other services, therefore we need to set network configuration (static IP for server), subnet mask, default gateway and DNS. Also, we need to add Active Directory Domain Services in Server Manager. We also created the default User so that we could join the domain from another Workstation.

<img width="1919" height="990" alt="Screenshot 2026-09-07 132648" src="https://github.com/user-attachments/assets/870a839e-f96c-4887-bf0d-4b8f0058535c" />
<img width="1919" height="993" alt="Screenshot 2026-09-07 132801" src="https://github.com/user-attachments/assets/b7d1d1bc-55f9-4a4d-96f7-eba38f71da63" />
<img width="1919" height="1018" alt="Screenshot 2026-09-07 135851" src="https://github.com/user-attachments/assets/9410bb7e-cd36-48d5-8d95-121f6799f8c2" />

Next, we have to configure the client machine's network configuration, otherwise, we could not connect it to the domain itself. The most important factor of this configuration is default gateway and DNS address - they need to be set to the IP address of the domain controller - in this case 192.168.56.10. This way the client machine will be able to communicate with DC and we will be able to join the domain.

<img width="1919" height="991" alt="Screenshot 2026-09-07 134341" src="https://github.com/user-attachments/assets/e42ab206-ec86-4e70-8cb9-a0feaf5d2bf4" />
<img width="1919" height="992" alt="Screenshot 2026-09-07 135938" src="https://github.com/user-attachments/assets/4b24ac5e-e467-466c-9b09-b2c48daa6c8d" />

# Building the Enterprise Directory Structure
Next step is building the Directory Structure for the company. We have decided for department-based approach, where we are sorting the organizational units by work departments in the company. Namely, the company has 4 different departments: IT, Finance, HR and Management. Each department has two separate organizational units; Workstations (devices in the network) and Users (people that are using those devices).

<img width="1919" height="988" alt="Screenshot 2026-09-08 112420" src="https://github.com/user-attachments/assets/873f0936-b431-4cbd-ba5b-08f57d51983d" />

### Automatization of User and Workstation Provisioning with PowerShell
Manual addition of users and workstations is possible but very time consuming and redundant approach. With PowerShell we have the ability to automate this process. In order to follow this approach, we need two things: csv file that contains employees/devices information and PowerShell script that will take the information from the document and process it into AD DS.

<img width="1919" height="990" alt="Screenshot 2026-09-08 112546" src="https://github.com/user-attachments/assets/d5c53fbb-4d65-4d6c-b2d4-f50a0d1b1534" />
<img width="1919" height="988" alt="Screenshot 2026-09-08 112638" src="https://github.com/user-attachments/assets/7c241c19-302a-4d0e-9bf0-e9a6216a2582" />
<img width="1919" height="993" alt="Screenshot 2026-09-08 112645" src="https://github.com/user-attachments/assets/eaaffae2-06e0-423d-9a45-5a1cde025010" />
<img width="1919" height="992" alt="Screenshot 2026-09-08 112658" src="https://github.com/user-attachments/assets/82740056-1875-4f40-9060-c098fdf02175" />

# Configuration of Group Policy Objects
GPOs are important for domain security and its practicality. In this case we will apply password policy rules, map network drive to all users from company through GPO and restrict execution of applications from temporary directories.

### Password policy rules
In this step we have enabled and adapted few important password policy rule changes. Maximum password age is set to 60 as a compromise between security and practicality - 30 days is relatively short period and it could become slightly more difficult for an employee to change and remember password every 30 days, therefore, we embraced 60 day mark as relatively secure and applicable period of password change. Minimum password length is set to 8 characters with also combining complexity requirements. As an additional security feature we set an account lockout duration timer, in order to escape brute-force threats.

<img width="1919" height="992" alt="Screenshot 2026-09-08 124849" src="https://github.com/user-attachments/assets/4667b441-e15d-4653-8868-9c73012309b5" />
<img width="1919" height="991" alt="Screenshot 2026-09-08 124858" src="https://github.com/user-attachments/assets/d46cfba9-23b7-42b8-9782-3239325e82d9" />


### Mapping network drive automatically through GPO
Similarly as adding new users into the organizational units, we have a problem of manual configuration of shared network drives. Redundant work in this case is substituted by Group Policy Object, where we have an opportunity to create a network drive and to assign it to specific user groups or organizational units in general. In this case we have shared the network drive with all users from the company, but it can also be applied to specific groups and departments. Likewise, to test the changes we have logged in as user from IT department of the company - John Doe - and we are able to see the network drive automatically mapped.

<img width="1919" height="987" alt="Screenshot 2026-09-08 123639" src="https://github.com/user-attachments/assets/fea781b9-ad4a-48cd-be40-cbb139b1e0a8" />
<img width="1919" height="985" alt="Screenshot 2026-09-08 123652" src="https://github.com/user-attachments/assets/16de95bb-b8f5-4e38-b98d-9a71608edb86" />

### AppLocker
An additional control that we have applied in this domain is AppLocker, where we have denied applications to run from temporary directories by path, of course, this feature is intended to stop some types of malicious files from running.

<img width="1919" height="987" alt="Screenshot 2026-09-08 131500" src="https://github.com/user-attachments/assets/41532d32-45ed-4485-9742-ab2e57c2827c" />

# Security telemetry and Threat monitoring
As we have mentioned earlier in the summary, due to environment baseline constraints we were not able to install Sysmon as intended for threat monitoring, but nevertheless, there were multiple points of security management available to be done.

### 1. Enabling Windows Advanced Audit Policies
In Group Policy Management we have configured next audit policies:
- Audit User Account Management (detects changes made to domain users or groups)
- Audit Kerberos Authentication Service (detects brute-force attacks and golden ticket attacks)
- Audit Kerberos Service Ticket Operations (detects Kerberoasting attacks)
- Audit Directory Service Changes (detects changes made to AD objects)
- Audit Logon (detects logon attempts)
- Audit Special Logon (detects logon attempts with administrative privilege)
- Audit Process Creation (detects successful creation of processes)
- Audit Audit Policy Change (detects attempts to disable logging)

<img width="1919" height="981" alt="Screenshot 2026-09-08 151956" src="https://github.com/user-attachments/assets/2e7d830e-d501-481a-92b9-a9b0248e42b4" />
