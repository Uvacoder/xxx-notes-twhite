> [!summary] 
> The initial design of the tool
> 


## How does it work?

- The tool should scan a network for open ports
- The tool should be able to recognize an SMB connection on an open port
- The tool should be able to determine what version of SMB the target machine is running
- The tools should be able to pivot through the share and grab a file and/or dump a script
- The script should remove traces of itself from the target machine once the payload is acquired
- The script should close the socket connection
- The script should then quit


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

