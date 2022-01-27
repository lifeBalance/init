# init
System and network administration.

## Network
### 01
**Question**: Get the list of the network interfaces of the machine without displaying any detail
for these interfaces. Only the list of names.

* **Explanation**: `ifconfig` (short for interface configuration) is a command-line tool for configuring the **network interface**. It provides a lot of options such as `up` (to enable an interface) or `down` (to disable it). The `-l` flag may be used to **list** all available interfaces on the system by **name**, with no other additional information.

* **Script**: Check `01`.

### 02
**Question**: Identify and display the Ethernet interface characteristics:
(a) Identify broadcast address
(b) Identify all IP adresses which are part of the same subnet

* **Explanation**: Usually in Linux/Unix systems, the **ethernet** interface is descriptively labeled with the prefix `eth`; so we'd have `eth0` for the first one, `eth1` for the second one, and so on. Another convention is to label them as `enp0s69`, where `en` stands for ethernet, then its **port** number, then its **slot** number. On the other hand, **wireless** interfaces are labeled with the `wl` (short for wireless) prefix, followed by its **port number** and its **slot number**, such as `wlp3s0`

In **macOS**, network interfaces start with `en0`, `en1`, and so on. From the name itself we don't have a clue if we're dealing with an **ethernet** or a **wireless** card, so we don't have a choice but to visit **System Preferences**, then **Network**, and on the sidebar select the interface we're interested on, and compare its **MAC address** with the output of `ifconfig`.

On systems with both an **ethernet** and **wireless** card, `en0` represents the **ethernet interface** and `en1` the **wireless one**. The **MAC address** is a bit confusingly preceded by the word `ether` for both **wireless** and **ethernet** cards, but that's due to the fact that **Wi-Fi** and **ethernet** both adhere to the **IEEE 802** standard for **local area networks**.

> On systems with just a wireless connection (like my MacBook Air), `en0` represents the **wireless interface**.

* **Script**: Check `02`. On the script I used a function named `get_broadcast_address` to get the broadcast address of the **ethernet interface** (`en0`). Then used the first **bytes** of this address to filter all the IP addresses in the same **subnet**.

### 03
**Question**: Identify the MAC address of the Wi-Fi card.

* **Explanation**: Since the school computers have both **ethernet** and **wireless** cards, in this case we need to specify the `en1` interface, `grep` the line with `ethernet`, and filter the second field.

* **Script**: Check `03`.

### 04
**Question**: Identifiy the default gateway in the routing table.

* **Explanation**: In computer networking, a **routing table**, is a data table stored in a router or a network host that lists the routes to particular network destinations. The routing table contains information about the topology of the network immediately around it. We can use the `netstat` command (short for network statistics) to get information about the network we're connected to. The `-r` flag is used to display the **routing table**, and the **default gateway** is clearly labeled as `default`, so we just have to do a bit of text filtering and done.
