# Invoke-shell
The sole purpose of this tool is for red-teamers and pentesters assessments.
This tool provides PowerShell reverse shell with AMSI bypass, PsExec download and persistence via WMI event subscription.

## Amsi Bypass
Almost all amsi bypass techniques have already been flagged and blocked. Therefore I choose Paul Laîné's technique which is still stealth. This technique manipulate amsi.dll by modifing the instructions of the Amsi ScanBuffer function, hence patching it in order to block the detection of "malicious" content.
<br />
For additional information: https://www.contextis.com/us/blog/amsi-bypass

## WMI Event Subscriptions for persistence
Windows Management Instrumentation (WMI) Event Subscription is one of various ways to establish persistence on a local machine.
WMI events run as an nt-authority\system, persists across reboots and Administrator privilege is required to use this technique.      
By default, the WMI service - Winmgmt is running and listening on tcp port 135.
A restart is required in order for the persistence to start.
Persistent WMI objects are stored in the subscription NameSpace in the WMI repository:
%windir%\System32\wbem\Repository\OBJECTS.DATA
![wmi1](https://user-images.githubusercontent.com/90933102/134239803-e1effd09-792f-4c3b-ad9f-dc8ae2a1c496.PNG)
After running the file, three new classes (Pentest-WMI) will be created in the repository:
#### __EventFilter
![wmi2](https://user-images.githubusercontent.com/90933102/134240537-b9e6d81b-46ff-49ad-8b64-5005d74e3eb1.PNG)
#### CommandLineEventConsumer
![wmi3](https://user-images.githubusercontent.com/90933102/134240862-69e2794f-435a-422b-a18c-2fd7ac9b1993.PNG)
This class stores and execute the payload
![wmi4](https://user-images.githubusercontent.com/90933102/134241471-5fc1b29c-9a09-4ffd-b5ea-bd8199484e10.PNG)
#### __FilterToConsumerBinding
![wmi5](https://user-images.githubusercontent.com/90933102/134243002-7e27bc29-2008-41d1-9c7d-8a15ac44a18a.PNG)

