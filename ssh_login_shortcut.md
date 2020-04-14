# How to login to a VM with a shortcut

1. Terminal
+ Create a ".command" file with the following contents.
```
ssh username@hostname -i <pem file>
```
+ Give +x permission for the ".command" file.

2. iTerm
+ Create a profile with configuring "Send text at start" to your ssh login details.
+ In Automator, create an Apllication with a AppleScript with the following:
```
tell application "iTerm"
	create window with profile "<profile name>"
end tell
```
+ Save the application changing the "File Format" to "Application".
+ Double click the above saved application and you can login to instance.

+ If you want to use the default profile the AppleScript would be:
```
tell application "iTerm"
	create window with default profile
end tell
```

It is possible to create a Quick Actions for the TouchBar using Automator.

