# Active Directory Security Lab
Automated Active Directory lab featuring GPO security baselines, Windows LAPS, advanced audit policies, and PowerShell identity provisioning.

# Tools
- Hypervisor - VirtualBox
- Server - Windows Server 2012 R2
- Client - Windows Server 2012 R2
- Logs - Sysmon
- SIEM - Splunk

# Network Diagram

# Installation and Configuration
The configuration of Active Directory lab in practice desires one server machine - that will serve as domain controller - and the other one - client machine to test the work of domain. As mentioned earlier, we use VirtualBox as a hypervisor together with Windows Server 2012 R2 installations (server is promoted to domain controller, while the client is not). Also, we are using Sysmon in order to generate logs for SPLUNK in the advanced phase of this lab where we are going to focus on security of the domain.

Domain controller needs to be configured in order to run other services, therefore we need to set network configuration (static IP for server), subnet mask, default gateway and DNS. Also, we need to add Active Directory Domain Services in Server Manager. We also created the default User so that we could join the domain from another Workstation.

<img width="1919" height="990" alt="Screenshot 2026-09-07 132648" src="https://github.com/user-attachments/assets/870a839e-f96c-4887-bf0d-4b8f0058535c" />
<img width="1919" height="993" alt="Screenshot 2026-09-07 132801" src="https://github.com/user-attachments/assets/b7d1d1bc-55f9-4a4d-96f7-eba38f71da63" />
<img width="1919" height="1018" alt="Screenshot 2026-09-07 135851" src="https://github.com/user-attachments/assets/9410bb7e-cd36-48d5-8d95-121f6799f8c2" />

Next, we have to configure the client machine's network configuration, otherwise, we could not connect it to the domain itself. The most important factor of this configuration is default gateway and DNS address - they need to be set to the IP address of the domain controller - in this case 192.168.56.10. This way the client machine will be able to communicate with DC and we will be able to join the domain.

<img width="1919" height="991" alt="Screenshot 2026-09-07 134341" src="https://github.com/user-attachments/assets/e42ab206-ec86-4e70-8cb9-a0feaf5d2bf4" />
<img width="1919" height="992" alt="Screenshot 2026-09-07 135938" src="https://github.com/user-attachments/assets/4b24ac5e-e467-466c-9b09-b2c48daa6c8d" />

# Building the Enterprise Directory Structure


