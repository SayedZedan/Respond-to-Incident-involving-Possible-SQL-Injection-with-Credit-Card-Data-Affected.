# Respond-to-Incident-involving-Possible-SQL-Injection-with-Credit-Card-Data-Affected.
Project Overview: In this project, a DLP (Data Loss Prevention) security appliance has alerted the security team to an attempt to exfiltrate credit card data. Management has requested confirmation on whether any customer credit card data was actually exfiltrated, and if so, to which destination, among other inquiries.
##  SQL Injection Attempts on the Server
After examining the provided project2.pcap file in Wireshark, there is clear evidence of SQL injection attempts targeting the server. By filtering for SQL-related traffic, multiple login attempts and SQL commands indicative of injection patterns were identified.

Steps Taken:

Open Wireshark: Launched Wireshark from the Kali terminal and loaded the project2.pcap file for analysis.

Filter for SQL Activity:

Used the search bar in Wireshark to filter for the term “sql”.
This quickly highlighted packets related to SQL commands and login attempts.
Analyze SQL Packets:

Examined each relevant packet in detail within the protocol tree to identify specific commands associated with SQL injection.
Observed several commands that fit known SQL injection patterns, confirming malicious activity.
The findings suggest a series of SQL injection attempts, validating the initial DLP alert regarding suspicious activity on the server.
![Screenshot (557)](https://github.com/user-attachments/assets/2ab03877-525b-46be-ba04-1d2164ede40b)
--------------------------------------------------------------------------------------------
## one SQL injection attempt was successful. The attacker used a command to invoke TFTP and download a file named keatron.exe onto the server.

Steps Taken:

Wireshark: Identified the SQL command where the attacker triggered TFTP to download keatron.exe.

Volatility: Loaded the memory dump, confirming keatron.exe as an active process in memory, proving successful transfer and execution.

This confirms that the attacker executed a successful SQL injection, leading to file transfer and process execution on the server.

![Screenshot (558)](https://github.com/user-attachments/assets/c8dc7ea5-f276-4714-884d-d4138eb6a6b1)
--------------------------------------------------------------------------------------------------------
## the file keatron.exe was transferred.

Steps Taken:

Wireshark: Applied a filter (tcp contains .exe) to identify any executable files transferred over the network.
Findings: The filter results show keatron.exe being transferred, confirming that it was indeed moved onto the server. See Image 1.4 for reference.
This file transfer aligns with the previous SQL injection activity and confirms a successful infiltration.

![Screenshot (559)](https://github.com/user-attachments/assets/6f76b866-fdd2-4abf-94d6-cfc97f38047f)
-------------------------------------------------------------------------------------------------
## there are connections between the attacker (192.168.248.200) and the victim (192.168.248.198).

Details:

The attacker initiated a connection to the victim on port 80.
The victim subsequently connected back to the attacker on port 5555.
These interactions can be verified in Wireshark by examining the Conversations feature under the Statistics menu.

Steps:

Filter Traffic: Create a filter to isolate traffic between IPs 192.168.248.198 and 192.168.248.200.
Refer to Figure 1.6.

Access Conversations: Navigate to Statistics > Conversations to list all connections. Refer to Figure 1.7.

Refine Display: Check the box for Limit to Display Filter and select TCP to see only filtered conversations. Refer to Figures 1.7 and 1.8.

These steps confirm the bidirectional connection, indicating possible data exfiltration following the SQL injection attack.

![Screenshot (560)](https://github.com/user-attachments/assets/373b7344-bd67-421b-ae67-1685e63b7ac7)
![Screenshot (561)](https://github.com/user-attachments/assets/8b3c1d3e-2922-4035-89e3-8e73142631a6)

------------------------------------------------------------------------------------------------------
Furthermore, multiple PowerShell commands were executed by the attacker. Evidence of this was found using the consoles plugin in Volatility. The commands included Powercat, which automates file transfers; Test-NetConnection, which checks host connectivity; IEX (New-Object System.Net.Webclient), used for establishing web connections; and more, which reads file contents.

The attacker interacted with a file named Gold.txt, which contained credit card numbers along with customer names and addresses. This file was extracted from memory using the dumpfiles plugin in Volatility.

Concluding the investigation, it was determined that credit card data had indeed been exfiltrated. The exfiltration occurred via the Gold.txt file, which was transferred to an external server with the IP address 18.216.211.10. This confirmed the successful data breach and exfiltration of sensitive customer information.








