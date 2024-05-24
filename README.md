# Honey User

## Introduction
This lab, from Antisyphon's Active Defense and Cyber Defense course, is designed to introduce the concept of honey users—decoy user accounts that are deliberately created to lure and detect malicious actors attempting to access unauthorized systems. By setting up these deceptive accounts, we can monitor and analyze unauthorized login attempts, providing valuable intelligence on attacker behavior and enhancing our overall security posture.

## Walkthrough
### STEP 1: In the Windows command prompt, I navigate to the correct directory. I run an automated script, *200-user-gen.bat*, to generate 200 users and the decoy Frank account.
![generate users](https://github.com/trixiahorner/honeyuser/blob/main/images/h1.png?raw=true)
<br>
<br>
### Step 2: Create a custom filter for the security event log.
Click on Windows start button and search for *Event View*. When in the Event Viewer, select *Windows Logs* > *Security* and then *Create Custom View*. 
![event viewer](https://github.com/trixiahorner/honeyuser/blob/main/images/h2.png?raw=true)
<br>
<br>
### Step 3: Select *XML*, *Edit query manually*, press *YES* on the alert box, and then finally replace the text query 
```
<QueryList>
  <Query Id="0" Path="Security">
    <Select Path="Security">* [EventData[Data[@Name='TargetUserName']='Frank']]</Select>
  </Query>
</QueryList>
```
![custom](https://github.com/trixiahorner/honeyuser/blob/main/images/h3.png?raw=true)
<br>
<br>
### Step 4: Press *OK*. When the Save Filter to Custom View box opens, name the filter *Frank* then press *OK*. 
When I click on new View I see 4 events associated with the Frank account being created: 
<br>
<br>
### STEP 5: Back in the Windows command prompt, I simulate a password spray on the local system
I create a rule that whenever someone accesses the Frank account, it will generate an alarm. So, if an attacker is on the network and attempting to authenticate, an alert will be generated. 
```
powershell
Set-ExecutionPolicy Unrestricted
Import-Module .\LocalPasswordSpray.ps1 
Invoke-LocalPasswordSpray -Password Winter2020 
```
![spray](https://github.com/trixiahorner/honeyuser/blob/main/images/h4.png?raw=true)
<br>
<br>
The password spray actually authenticated 6 people, but it didn’t get Frank. This actually doesn’t matter. For the alert to trigger, all that matters is that authentication was *ATTEMPTED*.
<br>
<br>
### STEP 6: In our Event Viewer, we can see the alerts! 
There is an *‘attempted authentication’* alert and an *‘enumeration’* alert.  This simulates an attacker performing a password spray and triggering our decoy user account. 
![alert](https://github.com/trixiahorner/honeyuser/blob/main/images/h5.png?raw=true)

## Conclusion
Honey users are decoy accounts designed to attract and detect unauthorized access attempts. They serve as an early warning system, alerting you to potential breaches and malicious activities.
