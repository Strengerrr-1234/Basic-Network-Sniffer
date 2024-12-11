# Basic-Network-Sniffer
Network sniffer in Python that captures and analyzes network traffic. This project will help you understand how data flows on a network and how network packets are structured.

This Python script is a simple network packet sniffer using the Scapy library. It captures and logs TCP connection details on a specified network interface. Below is a step-by-step explanation of the code:
### 1. Imports
```
import sys
from scapy.all import *
```
* `sys`: Provides access to command-line arguments and system-specific functions.

* `from scapy.all import *`: Imports all necessary modules from `Scapy`, a powerful Python library for network packet manipulation and analysis.
  
### 2. handle_packet(packet, log) Function
```
def handle_packet(packet, log):
    if packet.haslayer(TCP):
        src_ip = packet[IP].src
        dst_ip = packet[IP].dst
        src_port = packet[TCP].sport
        dst_port = packet[TCP].dport
        log.write(f"TCP Connection: {src_ip}:{src_port} -> {dst_ip}:{dst_port}\n")
```
*  Purpose: Processes each captured packet and logs details of TCP connections.
*  Steps:
  1.Checks if the packet contains the `TCP` layer using `packet.haslayer(TCP)`.
  2.Extracts:
   * ### Source IP: `packet[IP].src`
   * ### Destination IP: `packet[IP].dst`
   * ### Source Port: `packet[TCP].sport`
   * ### Destination Port: `packet[TCP].dport`
  3.Writes the TCP connection details `(src_ip:src_port -> dst_ip:dst_port)` to the log file.

### 3. `main(interface, verbose=False)` Function
```
def main(interface, verbose=False):
    logfile_name = f"sniffer_{interface}_log.txt"
    with open(logfile_name, 'w') as logfile:
        try:
            if verbose:
                sniff(iface=interface, prn=lambda pkt: handle_packet(pkt, logfile), store=0, verbose=verbose)
            else:
                sniff(iface=interface, prn=lambda pkt: handle_packet(pkt, logfile), store=0)
        except KeyboardInterrupt:
            sys.exit(0)
```
* ### Purpose:
   Sets up packet sniffing on a specified network interface and logs results.
* ### Steps:
  * ### Log File Setup:
    Creates a log file named `sniffer_<interface>_log.txt` in write mode.
  * ### `sniff` Function:
      * Captures packets on the given `interface`.
      * `prn`: Specifies a callback function to process each packet (`handle_packet` in this case).
      * `store=0`: Disables storage of packets in memory (to save memory during large captures).
      * `verbose`: Prints sniffing status messages if set to `True`.
      *  Handles `KeyboardInterrupt` gracefully (user presses `Ctrl+C`) by exiting the program cleanly.
### 4. Command-Line Arguments and Program Execution
```
if __name__ == "__main__":
    if len(sys.argv) < 2 or len(sys.argv) > 3:
        print("Usage: python sniffer.py <interface> [verbose]")
        sys.exit(1)
    verbose = False
    if len(sys.argv) == 3 and sys.argv[2].lower() == "verbose":
        verbose = True
    main(sys.argv[1], verbose)
```
* ### Purpose:
     Handles command-line arguments and starts the program.
* ### Steps:
   1. Checks the number of arguments:
      * Requires at least the network interface name.
      * Optionally accepts `verbose` as the third argument.
   2. Sets `verbose` mode if the third argument is `"verbose"`.
   3. Calls the `main()` function with the provided network interface and verbosity setting.

### 5. Execution Flow
* Run the script: `python sniffer.py <interface> [verbose]`
        * Replace  `<interface>` with the network interface to sniff on (e.g., `eth0` or `wlan0`).
        * Add `verbose` for detailed output.
* The script:
         1. Opens a log file.
         2. Sniffs packets on the specified interface.
         3.Logs TCP connection details.
         4. Stops gracefully when interrupted.

## Key Features:
 ### Custom Logging:
   Captures only TCP connections, making the output concise.
 ### Interface-specific sniffing:
  Sniffs on the user-specified network interface.
 ### Graceful Exit:
  Handles interruptions without crashing or losing data.
