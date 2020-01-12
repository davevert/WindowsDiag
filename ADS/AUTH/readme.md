# Microsoft CSS Authentication Scripts

## DESCRIPTION

This Data collection is for Authentication, Smart card and Credential provider scenarios.


## IMPORTANT

The authentication script is designed to collect information that will help Microsoft Customer Support Services (CSS) troubleshoot an issue you may be experiencing with Windows.
The collected data may contain Personally Identifiable Information (PII) and/or sensitive data, such as (but not limited to) IP addresses; PC names; and user names.
Once the tracing and data collection has completed, the script will save the data in a subdirectory from where this script is launched called "authlogs".
The "Authlogs" directory and subdirectories will contain data collected by the Microsoft CSS Authentication scripts.
This folder and its contents are not automatically sent to Microsoft.
You can send this folder and its contents to Microsoft CSS using a secure file transfer tool - Please discuss this with your support professional and also any concerns you may have.



## SCRIPT USAGE

Create a directory on the device where the tracing is going to run - example c:\ms
copy start-auth.txt and stop-auth.txt to target and rename with .bat extensions
From an elevated command prompt (as admin), navigate to the c:\ms directory and run start-auth.bat to start the tracing
Once you have created the issue or reproduced the scenario, please run stop-auth.bat from the same location to stop the tracing and collect the required data.
Data is collected into a subdirectory of the directory from where this script is launched, called "authlogs".
Please note that each script prompts for a Y input after the data collection information is displayed, to allow the script to run.


## SCRIPT USAGE NOTES

Network tracing is enabled by default in the scripts. If you do not want a network trace, please REM it out.
The default buffer size for network trace collection is 500MB, if you need a larger buffer then please edit the maxsize value in the following line of the start-auth bat file.

	netsh trace start traceFile=%_LOG_DIR%\Netmon.etl capture=yes maxsize=500 > NUL

By default, the Crypt, Ncrypt and Dpapi etl tracing is only enabled on client devices.
It is not enabled on server or DC's.
If you want to enable it on servers (ServerNT) or DC's (LanmanNT)  then you will need to remove the relevant REM statements from the following lines in the start-auth and stop-auth bat files.

Note: Please be aware that it should not be enabled on servers or DC's for long running data collection.


	REM if %%i equ ServerNT logman start CryptNCryptDpapi -pf %_CRYPT_DPAPI_TRACES_TMP% -o %_LOG_DIR%\CryptNcryptDpapi.etl -ets
	REM if %%i equ LanmanNT logman start CryptNCryptDpapi -pf %_CRYPT_DPAPI_TRACES_TMP% -o %_LOG_DIR%\CryptNcryptDpapi.etl -ets

	REM if %%i equ ServerNT logman.exe stop CryptNCryptDpapi -ets
	REM if %%i equ LanmanNT logman.exe stop CryptNCryptDpapi -ets


The SSL data collection is limited to 1024 MB's.
If you feel you need a larger size, then please edit the "max" value in the following line in the start-auth bat file

	logman start SSL -pf %_SSL_TRACES_TMP% -o %_LOG_DIR%\SSL.etl -ets -max 1024


The Winhhtp, Wininet and httpsys providers have limited tracing enabled by default
If you wish to enable more verbose tracing for any of these components, please amend the script as follows
Note: There tracing components can be verbose and may increase the ELT file size significantly.

	For Winhttp, amend the line with the GUID of B3A7698A-0C45-44DA-B73D-E181C9B5C8E6 to be as follows;

	echo {B3A7698A-0C45-44DA-B73D-E181C9B5C8E6} 0x7FFFFF 0xff

	for Wininet, amend the line with the GUID of 4E749B6A-667D-4c72-80EF-373EE3246B08 to be as follows;	

	echo {4E749B6A-667D-4c72-80EF-373EE3246B08} 0x7FFFFF 0xff

	For Httpsys, amend the line with the GUID of 20F61733-57F1-4127-9F48-4AB7A9308AE2 to be as follows;

	echo {20F61733-57F1-4127-9F48-4AB7A9308AE2} 0xFFFFFFFF 0xff


By default, certificate enrolment verbose debug logging is only enabled on client devices.
It is not enabled on server or DC's.
If you want to enable it on servers (ServerNT) or DC's (LanmanNT)  then you will need to remove the relevant REM statements from the following lines in the start-auth

	REM if %%i equ ServerNT certutil -setreg -f Enroll\Debug 0xffffffe3 > NUL
	REM if %%i equ LanmanNT certutil -setreg -f Enroll\Debug 0xffffffe3 > NUL
