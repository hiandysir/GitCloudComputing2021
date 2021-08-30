***
## Introduction

In this demo, we are going to show you how Eve can eavesdrop the traffic between Alice and Bob by overflowing the mac address table in the switches. This attack would work even if Eve is not connected to the same switch used by Alice and Bob. The topology we use in the demo is shown in the following figure:
![Topology for MacOverFlowAttack](http://yuba.stanford.edu/~huangty/cs144/images/MacOverflowAttack.png)


## Get Started
### Environment Setup
Please refer to [[Environment Setup]] for setting up the Mininet environment.

### Check Out Starter Code
```no-highlight
~$ cd ~
~$ git clone https://huangty@bitbucket.org/huangty/cs144_security.git
~/cs144_security$ cd cs144_security/
```

### Link POX into the Directory
Please refer to [[Environment Setup]] for downloading the POX network controller.
Assuming you have the "pox" directory under the home directory. 
```no-highlight
~/cs144_security$ ln -s ~/pox/
```

### Configure the Environment
```no-highlight
~/cs144_security$ bash ./config.sh
```

## Attack Demonstration
### Start POX Network Controller 
In one terminal:
```no-highlight
~/cs144_security$ ./run_pox.sh
```
This will start the POX network controller, which will emulate the behavior of a L2 learning switch. 

### Start Mininet Emulation 
In another terminal (if you are using a remote machine, make sure the X-forwarding is enabled.):
```no-highlight
~/cs144_security$ ./run.sh
```
This will start the Mininet network emulator and there will be terminals pops up for each of the nodes in the network. Close the terminals for switches and controllers, but keep the terminals for Alice, Bob and Eve. 

#### In Alice's Terminal:
Alice will now create some traffic by pinging Bob:
```no-highlight
alice@10.0.0.1:~/cs144_security$ ping 10.0.0.2
```
You should be able to see some output like the following: 
```no-highlight
alice@10.0.0.1:~/cs144_security$ping 10.0.0.2
PING 10.0.0.2 (10.0.0.2) 56(84) bytes of data.
64 bytes from 10.0.0.2: icmp_req=1 ttl=64 time=100 ms
64 bytes from 10.0.0.2: icmp_req=2 ttl=64 time=98.2 ms
64 bytes from 10.0.0.2: icmp_req=3 ttl=64 time=96.7 ms
64 bytes from 10.0.0.2: icmp_req=4 ttl=64 time=58.5 ms
64 bytes from 10.0.0.2: icmp_req=5 ttl=64 time=56.3 ms
...
```

#### In Eve's Terminal:
We will now run tcpdump to eavesdrop the traffic betweeh Alice (10.0.0.1) and Bob (10.0.0.2):
```no-highlight
eve@10.0.0.3:~/cs144_security$ sudo tcpdump -n host 10.0.0.1
```
Since the switch between Alice/Bob/Eve already learned about the address of Alice and Bob, it will not boradcast the packet and therefore Eve will not be able to see the packets between Alice and Bob.

Now, we will let Eve generate some ethernet packets with randomly generated source MAC address to overflow switches' MAC address table. To do so, let's create another terminal in screen for Eve by (Ctrl-a + c), then run the following command in the new screen:
```no-highlight
eve@10.0.0.3:~/cs144_security$ python attack.py 
```
You should be able to see Eve starts sending a lot of packets into the network. 

Back to Eve's first terminal (switch back by "ctrl+a 0). After the attack traffic overflowed swtiches' address table, switches will start to broadcast Alice and Bob's traffic and they should start showing up in Eve's tcpdump trace:
```no-highlight
eve@10.0.0.3:~/cs144_security$ sudo tcpdump -n host 10.0.0.1
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on eve-eth0, link-type EN10MB (Ethernet), capture size 65535 bytes
19:25:27.244766 ARP, Request who-has 10.0.0.2 tell 10.0.0.1, length 28
19:25:29.241754 IP 10.0.0.1 > 10.0.0.2: ICMP echo request, id 5031, seq 3, length 64
19:25:30.247182 IP 10.0.0.1 > 10.0.0.2: ICMP echo request, id 5031, seq 4, length 64
19:25:31.245341 IP 10.0.0.1 > 10.0.0.2: ICMP echo request, id 5031, seq 5, length 64
19:25:32.244768 IP 10.0.0.1 > 10.0.0.2: ICMP echo request, id 5031, seq 6, length 64
19:25:33.250965 IP 10.0.0.1 > 10.0.0.2: ICMP echo request, id 5031, seq 7, length 64
19:25:34.248849 IP 10.0.0.1 > 10.0.0.2: ICMP echo request, id 5031, seq 8, length 64
19:25:35.249185 IP 10.0.0.1 > 10.0.0.2: ICMP echo request, id 5031, seq 9, length 64
19:25:36.254024 IP 10.0.0.1 > 10.0.0.2: ICMP echo request, id 5031, seq 10, length 64
```
