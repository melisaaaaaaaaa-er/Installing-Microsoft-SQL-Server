# Installing-Microsoft-SQL-Server

<h2>Purpose</h2>

- Install a SQL server on windows-vm to serve as an additional endpoint for adversaries to discover, attack and generate logs on the system.

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/Installing-Microsoft-SQL-Server-Images/main/Lab%20Diagram.png"/>

#
<h2>Install SQL Server Evaluation</h2>

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/Installing-Microsoft-SQL-Server-Images/main/7.png"/>

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/Installing-Microsoft-SQL-Server-Images/main/8.png"/>

Download the SQL Server 2019 Evalutation edition (found at this link: https://www.microsoft.com/en-us/evalcenter/evaluate-sql-server-2019) ISO installer on the desktop of windows-vm.

You will need to register some credentials before you can download the .exe file. They don’t need to be real, just make them up.

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/Installing-Microsoft-SQL-Server-Images/main/9.png"/>

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/Installing-Microsoft-SQL-Server-Images/main/10.png"/>

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/Installing-Microsoft-SQL-Server-Images/main/11.png"/>

Select the ISO file option. Download it onto the desktop.

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/Installing-Microsoft-SQL-Server-Images/main/12.png"/>

#
<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/Installing-Microsoft-SQL-Server-Images/main/13.png"/>

Click the ISO file and select the setup file.

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/Installing-Microsoft-SQL-Server-Images/main/14.png"/>

Select the first option.

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/Installing-Microsoft-SQL-Server-Images/main/15.png"/>

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/Installing-Microsoft-SQL-Server-Images/main/16.png"/>

Configure the options shown in the screenshots.
- Select “Database Engine Services"
- Select “Mixed Mode” authentication
    - Input a password for the sa (system administrator) account
- Select “Add Current User” for “Specify SQL Server administrators”

The rest of the configurations can remain as default.

#
<h3>Install SQL Server Management Studio (SSMS)</h3>

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/Installing-Microsoft-SQL-Server-Images/main/17.png"/>

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/Installing-Microsoft-SQL-Server-Images/main/18.png"/>

Download and install SSMS from the following link - https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms.

You will be prompted to restart the VM. Select “Restart” and sign back into the machine with RDP again.

#
<h3>Enable SQL Server logs to be forwarded to Windows Event Viewer</h3>

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/Installing-Microsoft-SQL-Server-Images/main/19.png"/>

Follow the documentation from: Write SQL Server Audit events to the Security log - SQL Server | Microsoft Learn 

Go into the Registry Editor and navigate to HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Security

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/Installing-Microsoft-SQL-Server-Images/main/20.png"/>

Right-click and select “Permissions...” on the Security folder.

Add the NETWORK SERVICE account to the permissions list and enable it with Full Control.

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/Installing-Microsoft-SQL-Server-Images/main/21.png"/>

Enable auditing from the SQL server using the script provided.

Make sure to run the Command Prompt as Administrator.

#
<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/Installing-Microsoft-SQL-Server-Images/main/22.png"/>

Open SSMS and connect to the SQL server using the authentication credentials configured during the installation process.

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/Installing-Microsoft-SQL-Server-Images/main/23.png"/>

Right click the server to access the properties panel.

Go to “Security” and select “Both failed and successful logins” in the Login Auditing section.

Now logs will be generated for both failed and successful authentication attempts to the SQL server.

Right click the server and select “Restart” to implement the changes.

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/Installing-Microsoft-SQL-Server-Images/main/24.png"/>

Disconnect from the server and attempt to reconnect using faulty credentials to generate a failed authentication attempt log.

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/Installing-Microsoft-SQL-Server-Images/main/25.png"/>

Open the Event Viewer and navigate to Windows Logs and Applications.

Logs with the event ID of 18456 indicate a failed logon attempt.

#
<h3>Test Linux VM connectivity</h3>
<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/Installing-Microsoft-SQL-Server-Images/main/26.png"/>

<img src="https://raw.githubusercontent.com/melisaaaaaaaaa-er/Installing-Microsoft-SQL-Server-Images/main/27.png"/>

From your host machine, test connectivity to linux-vm through ping and SSH.

#
Though not strictly required, by this point of the lab you can leave the VMs on for 12 - 24 hours to allow unauthorized access/infiltration attempts from the Internet. These attempts can be viewed later in the lab where we analyze event logs. Again, this part is optional. If you implement this part, make sure to keep track of cost management for you subscription.
