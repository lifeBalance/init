# init
System and network administration.

## Network
### Exercise 01
**Question**: Get the list of the network interfaces of the machine without displaying any detail
for these interfaces. Only the list of names.

* **Explanation**: `ifconfig` (short for interface configuration) is a command-line tool for configuring the **network interface**. It provides a lot of options such as `up` (to enable an interface) or `down` (to disable it). The `-l` flag may be used to **list** all available interfaces on the system by **name**, with no other additional information.

* **Script**: Check `01`.

### Exercise 02
**Question**: Identify and display the Ethernet interface characteristics:
(a) Identify broadcast address
(b) Identify all IP adresses which are part of the same subnet

* **Explanation**: Usually in Linux/Unix systems, the **ethernet** interface is descriptively labeled with the prefix `eth`; so we'd have `eth0` for the first one, `eth1` for the second one, and so on. Another convention is to label them as `enp0s69`, where `en` stands for ethernet, then its **port** number, then its **slot** number. On the other hand, **wireless** interfaces are labeled with the `wl` (short for wireless) prefix, followed by its **port number** and its **slot number**, such as `wlp3s0`

In **macOS**, network interfaces start with `en0`, `en1`, and so on. From the name itself we don't have a clue if we're dealing with an **ethernet** or a **wireless** card, so we don't have a choice but to visit **System Preferences**, then **Network**, and on the sidebar select the interface we're interested on, and compare its **MAC address** with the output of `ifconfig`.

On systems with both an **ethernet** and **wireless** card, `en0` represents the **ethernet interface** and `en1` the **wireless one**. The **MAC address** is a bit confusingly preceded by the word `ether` for both **wireless** and **ethernet** cards, but that's due to the fact that **Wi-Fi** and **ethernet** both adhere to the **IEEE 802** standard for **local area networks**.

> On systems with just a wireless connection (like my MacBook Air), `en0` represents the **wireless interface**.

* **Script**: Check `02`. On the script I used a function named `get_broadcast_address` to get the broadcast address of the **ethernet interface** (`en0`). Then used the first **bytes** of this address to filter all the IP addresses in the same **subnet**.

### Exercise 03
**Question**: Identify the MAC address of the Wi-Fi card.

* **Explanation**: Since the school computers have both **ethernet** and **wireless** cards, in this case we need to specify the `en1` interface, `grep` the line with `ethernet`, and filter the second field.

* **Script**: Check `03`.

### Exercise 04
**Question**: Identifiy the default gateway in the routing table.

* **Explanation**: In computer networking, a **routing table**, is a data table stored in a router or a network host that lists the routes to particular network destinations. The routing table contains information about the topology of the network immediately around it. We can use the `netstat` command (short for network statistics) to get information about the network we're connected to. The `-r` flag is used to display the **routing table**, and the **default gateway** is clearly labeled as `default`, so we just have to do a bit of text filtering and done.

* **Script**: Check `04`.

### Exercise 05
**Question**: Identify the IP address of the DNS that responds to the following url: who.int.

* **Explanation**: A **DNS server** is a computer server that contains a database of public IP addresses and their associated hostnames, and in most cases serves to resolve, or translate, those **names** to **IP addresses** as requested. The `nslookup` command allows us to query Internet servers for information; if we pass a hostname (in this case `who.int`) as argument it will give us the IP to what that hostname resolves, but also the **Domain Name Server** used to translate the name to the IP. We just have to filter the result a bit to get just the IP address of the DNS.

* **Script**: Check `05`.

### Exercise 06
**Question**: Get the complete path of the file that contains the IP address of the DNS server
you’re using.

* **Explanation**: `resolv.conf` is the name of a a plain-text file used in various operating systems to configure the system's Domain Name System (DNS) resolver. In **macOS** this file is located under the `/etc` folder, hence the **full path** is `/etc/resolv.conf`.

* **Answer**: No script was asked, just a file (`06`) with the name mentioned above.

### Exercise 07
Query an external DNS server on the who.int domain name (ie.: google 8.8.8.8)

* **Explanation**: We can provide `nslookup` with a **second argument**: the **name server** to be used to check a **domain name** given as **first argument** (who.int). Without the second argument, `nslookup` will check the **default name server** (the one configured in `/etc/resolv.conf`).

* **Script**: Check `07`.

### Exercise 08
Find the provider of who.int

* **Explanation**: We first use the `nslookup` command to get the IP address of the who.int hostname. Then we feed that IP to the `whois` command. Finally, we add a bit of text processing magic and voila.

> A comfortable way of getting the IP of a domain is to google a site such as [hosting checker](https://hostingchecker.com/), but we chose the dark way ;-)

* **Answer**: Check `08` (it contains a script and the answer in a **comment**).

### Exercise 09
Find the external IP of 42.fr

* **Explanation**: Basically just using `nslookup` and a bit of text filtering fun (I reused the function of the last exercise).

* **Script**: Check `09`.

### Exercise 10
Identify the network devices between your computer and the who.int domain.

* **Explanation**: For that we can use the `traceroute` command, which prints the route that the packets take till the network host.

* **Script**: Check `10`.

### Exercise 11
Use the output of the previous command to find the name and IP address of the device that makes the link between you (local network) and the outside world.

* **Explanation**: From the output of `traceroute who.int` we can deduce that the **first hop** is the **IP** of the device that links our local network and the outside world. We can confirm that checking the **routing tables** using `netstat -rn`.

* **Answer**: NO script was asked. Check file `11` for the **answer**.

### Exercise 12
Find the IP that was assigned to you by dhcp server.

* **Explanation**: The `ipconfig` command communicates with the IPConfiguration agent to **retrieve** and **set** IP configuration parameters. In this case we need the IP that the DHCP server assigned to the interface that we use to connect to the network. Since we're connected using the **ethernet** interface (cable), we need to use the `getifaddr` to get the IP address of the `en0` interface.

> **DHCP** stands for Dynamic Host Configuration Protocol. All machines that belong to a network, are assigned **dynamic IP addresses** by a DHCP server.

* **Script**: Check `12`.

### Exercise 13
Thanks to the previous question and the reverse DNS find the name of your host.

* **Explanation**: We just have to use the command-line we wrote in the previous exercise as an argument to `nslookup` (it will only work if the network has set up a **reverse DNS lookup** or [rDNS](https://en.wikipedia.org/wiki/Reverse_DNS_lookup) for short).

> `networksetup -getcomputername` and `hostname` are easy ways of getting the name of our host.

* **Script**:  Check `13`.

### Exercise 14
What file contains the local DNS entries?

* **Explanation**: The [hosts file](https://en.wikipedia.org/wiki/Hosts_(file)) is where the system maps **hostnames** to **IP addresses**. In Linux/Unix systems it's located in the `/etc` folder, hence the **full path** to this file is `/etc/hosts`.

* **Script**: **NO SCRIPT**. Check file `14` for the **answer**.

### Exercise 15
Make the intra.42.fr address reroute to 46.19.122.85

* **Explanation**: To bypass the DNS server, we can add mapping of **hostnames** to **IP addresses** by adding additional lines to the `/etc/hosts` file.

* **Script**: Check file `15`.

> Since we need **superuser** privileges to edit the `/etc/hosts` file, in order to execute the command line in file `15` we would need to run: `sudo sh 15`

## System
### Exercise 01
In what file can you find the installed version of your Debian?

* **Answer**: In `/etc/debian_version`

### Exercise 02
What command can you use to rename your system?

* **Explanation**: In Linux, the `hostnamectl` command may be used to **query** and **change** the system hostname and related settings.

* **Script**: Check file `02`.

### Exercise 03
What file has to be modified to make it permanent?

* **Explanation**: That file is `/etc/hostname` which contains the name of the machine, used by applications that run locally. That name can be retrieved by the `hostname` command, and temporarily changed by the `hostnamectl` command.

* **Answer**: Check file `03`.

### Exercise 04
What command gives you the time since your system was last booted?

* **Explanation**: The `uptime` command-line utility shows how long system has been running.

* **Command**: Check file `04`.

### Exercise 05
Name the command that determines the state of the SSH service.

* **Explanation**: [systemd](https://en.wikipedia.org/wiki/Systemd) is a **service manager** for Linux operating systems. The `systemctl` command is used to control the systemd and service manager. There are a lot of **subcommands** to interact with `systemctl`, being `status` the one to check the state of a given service. That way, to check the **state of a service** (its `status`) we would use: `systemctl status service_name`.

* **Command**: Check file `05`.

### Exercise 06
Name the command that reboots the SSH service.

* **Explanation**: Once more, we just have to use the `systemctl` command, this time with the `restart` subcommand. When starting/stopping/restarting/reloading services we may be asked to authenticate our user.

* **Command**: Check file `06`.

### Exercise 07
Figure out the PID of the SSHD service.

* **Explanation**: To check the **process ID** of any **running program** (aka process), we could use the `ps` command (short for *process status*) which shows information about active processes. The `ax` flags (without prepending dash `-`) are commonly used to list **all processes**. Then we could `grep` the output to find information about the process we're interested in.

> Another way of checking the **PID** of a process is to check the `/var/run` directory, traditionally used by Linux systems to store volatile information about **running programs** (aka processes). So to check the process id of the `ssh` daemon we could simply cat the contents of the `/var/run/sshd.pid` file.

* **Command**: Check file `07`.

### Exercise 08
What file contains the RSA keys of systems that are authorized to connect via SSH?

* **Explanation**: The `authorized_keys` file in SSH specifies the SSH keys that can be used for logging into the user account for which the file is configured. It is a highly important configuration file, as it configures permanent access using SSH keys and needs proper management. This file can be found under the `$HOME/.ssh` folder.

> Usually this file is created by the user, so it may not exist by default.

* **Deduction**: Check file `08`.

### Exercise 09
What command lets you know who is connected to the System?

* **Explanation**: The `w` command shows who is logged on and what they are doing, meaning what processes are they running.

* **Command**: Check file `09`.

### Exercise 10
Name the command that lists the partition tables of drives?

* **Explanation**: The `fdisk` command is the commonly used to manipulate disk partition table. The `-l` (short for `--list`) lists the partition tables for the specified devices and then exit. If no device is specified, those mentioned in `/proc/partitions` (if that file exists) are used.

* **Command**: Check file `10`.

> The `fdisk` command must be run with **superuser** privileges, for example, `sudo fdisk -l`.

### Exercise 11
Name the command that displays the available space left and used on the system in an humanly understandable way.

* **Explanation**: The `df` command-line utility displays statistics about the amount of free disk space on the specified filesystem or on the filesystem of which file is a part. The `-h` and `-H` options both produce **human readable** output. The difference between the two is that `-h` uses **base 2** units (kibibytes, mebibytes, etc), whereas `-H` uses **base 10** units (kilobytes, megabytes, etc).

* **Command**: Check file `11`.

### Exercise 12
Figure out the exact size of each folder of /var in a humanly understandable way followed by the path of it.

* **Explanation**: The `du` command-line utility displays **disk usage** statistics. The `-h` option produces **human readable** output in **base 10** units (kilobytes, megabytes, etc). The `-s` option (short for `--summarize`) is used to show just a total for each argument. If we want to target only the **subfolders** of `/var` we can use `/var/*` (note the asterisk).

* **Command**: Check file `12`.

### Exercise 13
Name the command that find, in real time, currently running processes.

* **Explanation**: The [top command](https://en.wikipedia.org/wiki/Top_(software)) produces an ordered list of running processes and updates it periodically.

* **Command**: Check file `13`.

