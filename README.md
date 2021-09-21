# Invoke-shell
The sole purpose of this tool is for red-teamers and pentesters assessments.
This tool provides PowerShell reverse shell with AMSI bypass, PsExec download and persistence via WMI event subscription.

# Amsi Bypass
Almost all amsi bypass techniques have already been flagged and blocked. Therefore I choose Paul Laîné's technique. This technique manipulate amsi.dll by modifing the instructions of the Amsi ScanBuffer function, hence patching it in order to block the detection of "malicious" content.
For additional information: https://www.contextis.com/us/blog/amsi-bypass

# WMI Event Subscriptions for persistence
Windows Management Instrumentation (WMI) Event Subscriptions is one of various ways to establish persistence on a local machine.
