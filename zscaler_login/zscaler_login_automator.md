# Applescript to automate Zscaler Private Access loging

## **Note: I am not sure whether this script will work in other systems since the element identifiers may differ in other systems.**

I have to use Zscaler for my day to day work. The headace is, I have to prove myself by login to Zscaler everytime the session expires.

Getting to the login menu of Zscaler PA requires a lot of clicking. Depending on the configuration by the Zscaler admin, it is required to login every few hours. Zscaler doesn't let me know that I have to relogin until an app tries to use PA.
So, I have to,
1. Use the PA app and notified that I have to relogin.
2. Relogin to Zscaler PA.

I have to do this every morning and I thought to automate this because it takes 1~2 minutes for the whole process. I am sharing the AppleScript I created for others as reference.

Honestly, it was not easy creating this script. Zscaler works as an "agent app" and it doesn't have a Scripting Dictionary. Also, it doesn't answer to normal Applescript commands. I think they don't provide this on purpose.

The following script does the following:
1. Open Microsoft Remote Desktop and try to initiate a RDP connection.
2. Open Zscaler app and move to the authentication window through User Interface Scripting.

You need to export your RDP connection and save it as a file.

```
set ZS_USERNAME to "<your Zscaler account username>" --Zscaler username
set ZS_LOGIN_TIME to "5" --Waiting time until Zscaler login window
set RDP_NAME to "<your rdp file name>" --Saved RDP shortcut name
set RDP_PATH to "<path to your rdp file>--Path to the saved RDP shortcut
set RDP to quoted form of (RDP_PATH & RDP_NAME & ".rdp") --Set RDP path

--Open RDP connection and close the error message window
do shell script ("open " & RDP)
delay 0.3
tell application "System Events"
	tell application process "Microsoft Remote Desktop"
		delay 0.5
		if exists sheet 1 of window 1 then
			click button "Close" of sheet 1 of window 1
		else
			display dialog "Check whether PA login required."
		end if
	end tell
end tell

--Open Zscaler window
tell application "System Events" to tell process "Zscaler"
	set frontmost to true
	ignoring application responses
		click menu bar item 1 of menu bar 2
	end ignoring
end tell

--Had to go with killall otherwise it was so slow
do shell script "killall System\\ Events"
delay 0.3
tell application "System Events" to tell process "Zscaler"
	try
		click menu item 1 of menu 1 of menu bar 2
	end try
end tell


--Click Zscaler PA button and full authentication details
tell application "System Events" to tell process "Zscaler"
	set frontmost to true
	tell window 1
		click button 4
		delay 0.5
		click button "Reauthenticate" of group 1
		delay ZS_LOGIN_TIME
		tell scroll area 1
			tell UI element 1
				tell group 1
					set value of text field 1 of group 1 of group 5 to ZS_USERNAME
					keystroke tab
				end tell
			end tell
		end tell
	end tell
end tell
```

It is possible to automate run this script everytime you login to your Mac.

[How to create Automator app](https://www.instructables.com/id/How-to-make-apps-automator/)

[How to run app at startup](https://www.idownloadblog.com/2015/03/24/apps-launch-system-startup-mac/)
