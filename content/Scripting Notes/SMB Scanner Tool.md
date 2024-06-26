> [!summary] 
> The initial design of the tool
> 


## How does it work?

- ~~The tool should scan a network for open ports~~
- ~~The tool should be able to recognize an SMB connection on an open port~~
- ~~The tool should be able to determine what version of SMB the target machine is running~~
- ~~The tools should be able to pivot through the share and grab a file and/or dump a script~~
- ~~The script should remove traces of itself from the target machine once the payload is acquired~~
- ~~The script should close the socket connection~~
- ~~The script should then quit~~
- The script should build upon `nmap` and `rustscan`


> [!important] 
> The above list assumes the tool should do more than is necessary. Simplifying the tool is the best course of action. Simple scripts for simple tasks.
> 


## What should the script do?

The script should do ***only four*** things

- List server shares
- Find null authentication
- Enumerate shares
- List permissions on each shared directory

## Implementation Details

> [!example] 
> Libraries: 
> - impacket
> - argparse
> - colorama
> - socket
> - urllib
> - os
> - datetime

## Common SMB Ports

- 135
- 139
- 445

## Enumeration

> [!question] 
> What are we looking for on an open SMB share?
> 

- Check for anonymous access
- Credentials
- List of shares
- Usernames
- Groups
- Permissions
- Policies
- Services
- etc...
