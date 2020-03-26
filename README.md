# PowerShell-Domain-Controller-Audit-Script

A PowerShell script which can be copied/pasted into a PowerShell console window and retrieve auditable domain controller configuration settings. This script only runs a series a queries (it does not make any modifications) and then creates a folder with a text and html document on the user's desktop named after the computer name which can then be zipped and uploaded as supporting documentation.

This script needs to be run on the domain controller. Simply copy the entire blob of text below and paste into a PowerShell window in administrative mode.

Search PowerShell, right click the Windows PowerShell app, and select "run as administrator".

New-Item -ItemType directory -Path "$($env:USERPROFILE)\Desktop\Domain Controller Audit" ; echo "SYSTEM INFORMATION:" | Out-File "$($env:USERPROFILE)\Desktop\Domain Controller Audit\\$env:computername.txt" ; systeminfo | Out-File "$($env:USERPROFILE)\Desktop\Domain Controller Audit\\$env:computername.txt" -append ; echo "ANTIVIRUS STATUS:" | Out-File "$($env:USERPROFILE)\Desktop\Domain Controller Audit\\$env:computername.txt" -append; Get-MpComputerStatus | Out-File "$($env:USERPROFILE)\Desktop\Domain Controller Audit\\$env:computername.txt" -append ; echo "AUDIT POLICY SETTINGS:" | Out-File "$($env:USERPROFILE)\Desktop\Domain Controller Audit\\$env:computername.txt" -append ; auditpol.exe /get /category:* | Out-File "$($env:USERPROFILE)\Desktop\Domain Controller Audit\\$env:computername.txt" -append ; echo "INSTALLED UPDATES:" | Out-File "$($env:USERPROFILE)\Desktop\Domain Controller Audit\\$env:computername.txt" -append ; wmic qfe list | Out-File "$($env:USERPROFILE)\Desktop\Domain Controller Audit\\$env:computername.txt" -append ; echo "INSTALLED PROGRAMS:" | Out-File "$($env:USERPROFILE)\Desktop\Domain Controller Audit\\$env:computername.txt" -append ; wmic product get name,version | Out-File "$($env:USERPROFILE)\Desktop\Domain Controller Audit\\$env:computername.txt" -append ; echo "SHARES:" | Out-File "$($env:USERPROFILE)\Desktop\Domain Controller Audit\\$env:computername.txt" -append ; net share | Out-File "$($env:USERPROFILE)\Desktop\Domain Controller Audit\\$env:computername.txt" -append ; echo "PASSWORD POLICY SETTINGS:" | Out-File "$($env:USERPROFILE)\Desktop\Domain Controller Audit\\$env:computername.txt" -append ; net accounts | Out-File "$($env:USERPROFILE)\Desktop\Domain Controller Audit\\$env:computername.txt" -append ; Get-ADDefaultDomainPasswordPolicy | Out-File "$($env:USERPROFILE)\Desktop\Domain Controller Audit\\$env:computername.txt" -append ; Get-ADFineGrainedPasswordPolicy -filter * | Out-File "$($env:USERPROFILE)\Desktop\Domain Controller Audit\\$env:computername.txt" -append ; echo "DOMAIN ADMINS:" | Out-File "$($env:USERPROFILE)\Desktop\Domain Controller Audit\\$env:computername.txt" -append ; Get-ADGroupMember -Identity "Domain Admins" -Recursive | %{Get-ADUser -Identity $_.distinguishedName} | Select Name, Enabled | Out-File "$($env:USERPROFILE)\Desktop\Domain Controller Audit\\$env:computername.txt" -append ; echo "ENTERPRISE ADMINS:" | Out-File "$($env:USERPROFILE)\Desktop\Domain Controller Audit\\$env:computername.txt" -append ; Get-ADGroupMember -Identity "Enterprise Admins" -Recursive | %{Get-ADUser -Identity $_.distinguishedName} | Select Name, Enabled | Out-File "$($env:USERPROFILE)\Desktop\Domain Controller Audit\\$env:computername.txt" -append ; echo "LOCAL ADMINISTRATOR AND GUEST STATUS:" | Out-File "$($env:USERPROFILE)\Desktop\Domain Controller Audit\\$env:computername.txt" -append ; Get-LocalUser | Out-File "$($env:USERPROFILE)\Desktop\Domain Controller Audit\\$env:computername.txt" -append ; get-gporeport -all -reporttype HTML -path "$($env:USERPROFILE)\Desktop\Domain Controller Audit\DomainGPOs.html"
