* What is this? * 
A program like a firewall that monitors TCP connections on Windows and disconnects when certain conditions are met. Unlike regular firewalls, you can set conditions specific to the transfer amount and transfer speed. 

* How to use * 
Double click TinyTCPFirewall.exe to start it. After that it works without permission until it ends. In addition to being displayed in the DOS window, the action log is saved as log.txt in the same directory as the program. 

* Attention * 
· Administrator authority is required for operation. If you do not have 	administrator privileges, it will not start.

· The local IP address to be monitored is detected automatically, but if the address you want to target can not be selected for reasons such as when there are 	multiple NICs etc., you can directly connect the IP address to the MyIPAddress key in the Global section of the Rules.ini file You can specify it.

· Although it depends on the OS environment and settings, for monitoring downstream 	(reception) direction, if this program (TinyTCPFirewall.exe) is added to the OS firewall exception, packets can not come. Please note that Windows standard firewall does not function properly if environment variable such as %USERPROFILE% is included in executable file path specification. 

* How to write rules * 
Set rules for setting disconnection rules in the Rules.ini file in the same directory as the executable file. Rules must be written in sections beginning with Rule. The keys used in the rules are explained below. 

ExeName key (string value, required) 
Specify the exe file name of the target program. Please note that it is case 	sensitive. 
***Example) ExeName = "foo.exe" 
-> It targets programs with executable file name called foo.exe.
 
When key (integer value, default value = 0) 
Specify the timing to apply the rule as the number of seconds since communication start. If 0 is specified, the rule is always applied. If you specify a value other than 0, the rule will be evaluated only once at that time. 
***Example) When = 10 
-> This rule is applied when 10 seconds have elapsed from the start of communication. 

ApplyAfter key (integer value, default value = 0 or value specified in the Global section) 
Specify the length of time, in seconds, that the rule is not applied immediately after communication starts. When When is 0, it has a meaning, and when a value other than 0 is specified for When, the value of this key has no meaning. It can also be described in the Global section, in which case you define the default value if the key does not exist. 
***Example) ApplyAfter = 5 
-> Do not apply this rule until 5 seconds elapse from communication start. 

DownloadRateUpper key (integer value, default value = 0) 
Specify the upper limit of the average reception speed in KB/s. If the condition is satisfied, the connection is disconnected. If 0 is specified, this condition is ignored. 
***Example) DownloadRateUpper = 100 
-> When the average reception speed exceeds 100 KB/s, the connection is disconnected. 

DownloadAmountUpper (integer value, default value = 0) 
Specify the upper limit of the reception amount in KB units. If the condition is satisfied, the connection is disconnected. If 0 is specified, this condition is ignored. 
***Example) DownloadAmountUpper = 1000 
-> If the received amount exceeds 1000 KB, disconnect the connection. 

DownloadRateLower key (integer value, default value = 0) 
Specify the lower limit of the average reception speed in KB/s. If the condition is satisfied, the connection is disconnected. If 0 is specified, this condition is ignored. 
***Example) DownloadRateLower = 100 
-> When the average reception speed falls below 100 KB/s, the connection is disconnected.
 
DownloadAmountLower (integer value, default value = 0) 
Specifies the lower limit of the reception amount in KB units. If the condition is satisfied, the connection is disconnected. If 0 is specified, this condition is ignored. 
***Example) DownloadAmountLower = 1000 
-> If the reception volume is less than 1000 KB, disconnect the connection. 

UploadRateUpper key (integer value, default value = 0) 
Specify the upper limit of average transmission speed in KB/s. If the condition is satisfied, the connection is disconnected. If 0 is specified, this condition is ignored. 
***Example) UploadRateUpper = 100 
-> When the average transmission speed exceeds 100 KB/s, the connection is disconnected.
 
UploadAmountUpper (integer value, default value = 0) 
Specify the upper limit of transmission amount in KB units. If the condition is satisfied, the connection is disconnected. If 0 is specified, this condition is ignored. 
***Example) UploadAmountUpper = 1000 
-> If the transmission exceeds 1000 KB, disconnect the connection. 

UploadRateLower key (integer value, default value = 0) 
Specify the lower limit of average transmission speed in KB/s. If the condition is satisfied, the connection is disconnected. If 0 is specified, this condition is ignored. 
***Example) UploadRateLower = 100 
-> When the average transmission speed falls below 100 KB/s, the connection is disconnected.

UploadAmountLower (integer value, default value = 0) 
Specifies the lower limit of the transmission amount in KB units. If the condition is satisfied, the connection is disconnected. If 0 is specified, this condition is ignored. 
***Example) UploadAmountLower = 1000 
-> If the transmission volume is less than 1000 KB, disconnect the connection. 

**********************************************

Here are some examples of actually writing rules. If multiple conditions of Amount / Rate are specified, the AND of each condition is evaluated. 

***Example 1) 
Disconnect a connection whose transmission speed exceeds 250 KB/s in 5 seconds from the start of communication with hoge.exe 

[Rule 1] 
ExeName = "hoge.exe" 
When = 5 
UploadRateUpper = 250 

***Example 2) 
Total at hoge.exe 

[Rule 2] 
ExeName = "hoge.exe" 
When = 0 
DownloadAmountUpper = 10000 
UploadAmountLower = 1000 

When specifying "0" other than 0, disconnect the connection with Rate type The meaning is (almost) the same as specifying the condition with the key of "Amount" key. Please use one that is easy to specify. 


* Black List * 
There is a key called BlackListThreshold in the Global section of the ini file. If you specify a value other than 0 for this, IP addresses whose number of disconnections has reached the specified number will be added to the blacklist and will be immediately disconnected. The IP address added to the black list is recorded in a file called blacklist.txt in addition to the log. This file has a format compatible with PeerBlock etc. (explanation: start IP address - end IP address), and it can be given directly as input to those files. 
Also, if you specify a value other than 0 for the LoadBlackList key in the Global section, the blacklist will be initialized with the contents of blacklist.txt at startup. This is useful when you want to inherit the contents of the black list and operate. 


* Other options * 
If you set the DisableCloseButton key to a value other than 0, the close button will be invalidated. In this case, you can exit with Ctrl + C. 

* Distributor *
https://github.com/tmkk/TinyTCPFirewall