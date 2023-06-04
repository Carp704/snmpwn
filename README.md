## Credit
All credit belongs to [hatlord](https://github.com/hatlord). This is an edit of their work to add design and latency/timeout features.

## Overview
SNMPwn is an SNMPv3 user enumeration and dictionary attack tool. It is a legitimate security tool designed to be used by security professionals and penetration testers against hosts you have permission to test. It takes advantage of the fact that SNMPv3 systems will respond with "Unknown user name" when an SNMP user does not exist, allowing us to cycle through large lists of users to find the ones that do.

## What does it do?
- Checks that the hosts you provide are responding to SNMP requests.
- Enumerates SNMP users by testing each in the list you provide. Think user brute forcing.
- Performs a dictionary attack on the server with the enumerated accounts and your list of passwords and encryption passwords. No need to attack the entire list of users, only live accounts.
- Attacks all the different protocol types:
	- No auth no encryption (noauth)
    - Authentication, no encryption (authnopriv)
    - Authentication and encryption (All types supported, MD5, SHA, DES, AES) - (authpriv)
    
## Notes for usage
Built and tested on Kali Linux 2023.1. Should work on any Linux platform. SNMPWN does not work currently on Mac OSX due to the stdout messages for snmpwalk on OSX being different. This script basically wraps snmpwalk. The version of snmpwalk used was 5.9.3.

## Install
````
git clone https://github.com/Carp704/snmpwn.git
cd snmpwn
gem install bundler  
bundle install
./snmpwn.rb
````
Built for Ruby 3.1.x. Older versions of Ruby may work, but older than 1.9 may not.

## Run  
You need to provide the script a list of users, a hosts list, a password list and an encryption password list. Basic users.txt and passwords.txt files are included. You could use passwords.txt for your encryption list also. I would recommend generating one specific to the organisation you are pen testing.
The command line options are available via `--help` as always and should be clear enough. The only ones I would make specific comment on are:
`--showfail` - This will show you all password attack attempts, both successful and failed. It clutters the console output though, so if you do not choose this option you will get a spinning progress indicator instead.
`--timeout` - This is the timeout in milliseconds for the command response, which in this case is snmpwalk. It is set to 0.3 by default, which is 300 milliseconds. If you are testing hosts across a slow link you are going to want to extend this. I wouldn't personally go lower than 300 or results may become unreliable.

````
./snmpwn.rb --hosts hosts.txt --users users.txt --passlist passwords.txt --enclist passwords.txt
````
