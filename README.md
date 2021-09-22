# Invoke-shell
This tool provides PowerShell reverse shell with AMSI bypass and several additional features such as PsExec download and persistence via WMI event subscription.
<br /> The sole purpose of this tool is for red-teamers and pentesters assessments.


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
<br />
<img src=https://user-images.githubusercontent.com/90933102/134239803-e1effd09-792f-4c3b-ad9f-dc8ae2a1c496.PNG width="405" height="350">
<br />
After running the file, three new classes (Pentest-WMI) will be created in the repository:

#### __EventFilter
<img src=https://user-images.githubusercontent.com/90933102/134240537-b9e6d81b-46ff-49ad-8b64-5005d74e3eb1.PNG width="532" height="350">

#### CommandLineEventConsumer
<img src=https://user-images.githubusercontent.com/90933102/134240862-69e2794f-435a-422b-a18c-2fd7ac9b1993.PNG width="516" height="350">
This class stores and execute the payload
<img src=https://user-images.githubusercontent.com/90933102/134241471-5fc1b29c-9a09-4ffd-b5ea-bd8199484e10.PNG width="487" height="300">

#### __FilterToConsumerBinding
<img src=https://user-images.githubusercontent.com/90933102/134243002-7e27bc29-2008-41d1-9c7d-8a15ac44a18a.PNG width="536" height="350">

## Compiling
Before compiling the script, IP (and port if different from 443) must be changed.
<br />
There are several methods to compile ps1 to executables, however IExpress utility is a "Living off the Land". IExpress is a built-in Windows application that’s typically used for packaging files or creating software installers. <br />
IExpress must be opened with an admin: <br />
<img src=https://user-images.githubusercontent.com/90933102/134406152-dda9b034-4df6-48fb-9497-2f4e5e7e3f21.PNG width="502" height="200"> <br /> <br />
Select <b> Create new Self-Extraction Directive File </b> option and click Next:<br />
<img src=https://user-images.githubusercontent.com/90933102/134407543-977cf679-80d8-4acf-9a97-9a3f26b6a673.PNG width="436" height="350">
<br /> <br />
Select the <strong>Extract files and run an installation command</strong> <br />
<img src=https://user-images.githubusercontent.com/90933102/134408142-0bd5ecc5-b91e-4899-bcf4-00fdf50f26ca.PNG width="444" height="350">
  <br /> <br />
Write a <b>Package Title</b> and click Next: <br />
<img src=https://user-images.githubusercontent.com/90933102/134409537-eb9c83bc-31c5-4541-80c5-035e698cbf02.PNG width="438" height="350">
<br /> <br />
Next, choose <b>No prompt</b>: <br />
<img src=https://user-images.githubusercontent.com/90933102/134410110-d307c0ca-739b-4aaa-8d7f-c39a55a7b69b.PNG width="445" height="350">
<br /> <br />
In the License Agreement choose <b>Do not display a license</b> option and click Next: <br />
<img src=https://user-images.githubusercontent.com/90933102/134410658-ebee1c5c-b4cd-4df1-930f-4c9f16885e3a.PNG width="448" height="350"> 
<br /> <br />
Now click on <b>Add</b> and select the reverse.ps1 file: <br />
<img src=https://user-images.githubusercontent.com/90933102/134411928-0921da31-f048-4f67-a059-7b207566e382.PNG width="447" height="350"> 
<br /> <br />
On the Install Program window, write <b>powershell.exe -ExecutionPolicy Bypass -File reverse.ps1</b> and click Next:
<img src=https://user-images.githubusercontent.com/90933102/134415330-a694f266-fa05-4e07-8b5c-6a75b3f1851f.PNG width="446" height="350"> 
<br /> <br />
As the exe file is supposed to run in the background, choose <b>Hidden</b> and click <b>Next</b>:
<img src=https://user-images.githubusercontent.com/90933102/134416596-9cb48117-a6c0-4b48-a171-6d46f05376a7.PNG width="451" height="350">
<br /> <br />
Select <b>No message</b> and <b>Next</b>: <br />
<img src=https://user-images.githubusercontent.com/90933102/134417150-49b32d83-9221-4e09-8554-528ab5322e9c.PNG width="447" height="350">
<br /> <br />
Now browse the path and write the exe file's name (pleaserunme.exe) you are going to create. Enable both options and click Next:
<br />
<img src=https://user-images.githubusercontent.com/90933102/134417824-59281331-3213-4230-975e-a881de6d42be.PNG width="445" height="350">
<br /> <br />
No restart is needed:<br />
<img src=https://user-images.githubusercontent.com/90933102/134418930-859d9c14-99f6-4420-a6fd-4ae5848afd1d.PNG width="448" height="350">
<br /> <br />
Now select <b>Save Self Extraction Directive (SED) file</b> and click <b>Next</b>. <br />Saving a SED file allows you to modify any options you’ve provided throughout this wizard at a later time:
