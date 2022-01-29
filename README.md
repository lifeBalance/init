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

> In case of doubt, the `network -listallhardwareports` command lists all the network interface names along with their **MACs**.

* **Script**: Check `02`. On the script I used a function named `get_broadcast_address` to get the broadcast address of the **ethernet interface** (`en0`). Then used the first **bytes** of this address to filter all the IP addresses in the same **subnet**.

### Exercise 03
**Question**: Identify the MAC address of the Wi-Fi card.

* **Explanation**: Since the school computers have both **ethernet** and **wireless** cards, in this case we need to specify the `en1` interface, `grep` the line with `ethernet`, and filter the second field.

> On systems with just a wireless connection (like my MacBook Air), `en0` represents the **wireless interface**.

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

* **Deduction**: No script was asked, just a file (`06`) with the name mentioned above.

### Exercise 07
Query an external DNS server on the who.int domain name (ie.: google 8.8.8.8)

* **Explanation**: We can provide `nslookup` with a **second argument**: the **name server** to be used to check a **domain name** given as **first argument** (who.int). Without the second argument, `nslookup` will check the **default name server** (the one configured in `/etc/resolv.conf`).

* **Script**: Check `07`.

### Exercise 08
Find the provider of who.int

* **Explanation**: We first use the `nslookup` command to get the IP address of the who.int hostname. Then we feed that IP to the `whois` command. Finally, we add a bit of text processing magic and voila.

> A comfortable way of getting the IP of a domain is to google a site such as [hosting checker](https://hostingchecker.com/), but we chose the dark way ;-)

* **Deduction**: Check `08` (it contains a script and the answer in a **comment**).

### Exercise 09
Find the external IP of 42.fr

* **Explanation**: Basically just using `nslookup` and a bit of text filtering fun (I reused the function of the last exercise).

* **Command Output**: Check `09`.

### Exercise 10
Identify the network devices between your computer and the who.int domain.

* **Explanation**: For that we can use the `traceroute` command, which prints the route that the packets take till the network host.

* **Script**: Check `10`.

### Exercise 11
Use the output of the previous command to find the name and IP address of the device that makes the link between you (local network) and the outside world.

* **Explanation**: From the output of `traceroute who.int` we can deduce that the **first hop** is the **IP** of the device that links our local network and the outside world. We can confirm that checking the **routing tables** using `netstat -rn`.

* **Deduction**: Check file `11` for the **answer**.

### Exercise 12
Find the IP that was assigned to you by dhcp server.

* **Explanation**: The `ipconfig` command communicates with the IPConfiguration agent to **retrieve** and **set** IP configuration parameters. In this case we need the IP that the DHCP server assigned to the interface that we use to connect to the network. Since we're connected using the **ethernet** interface (cable), we need to use the `getifaddr` to get the IP address of the `en0` interface.

> **DHCP** stands for Dynamic Host Configuration Protocol. All machines that belong to a network, are assigned **dynamic IP addresses** by a DHCP server.

* **Script**: Check `12`.

### Exercise 13
Thanks to the previous question and the reverse DNS find the name of your host.

* **Explanation**: We just have to use the command-line we wrote in the previous exercise as an argument to `nslookup` (it will only work if the network has set up a **reverse DNS lookup** or [rDNS](https://en.wikipedia.org/wiki/Reverse_DNS_lookup) for short).

> `networksetup -getcomputername` and `hostname` are easy ways of getting the name of our host.

* **Command Output**:  Check `13`.

### Exercise 14
What file contains the local DNS entries?

* **Explanation**: The [hosts file](https://en.wikipedia.org/wiki/Hosts_(file)) is where the system maps **hostnames** to **IP addresses**. In Linux/Unix systems it's located in the `/etc` folder, hence the **full path** to this file is `/etc/hosts`.

* **Deduction**: Check file `14` for the **answer**.

### Exercise 15
Make the intra.42.fr address reroute to 46.19.122.85

* **Explanation**: To bypass the DNS server, we can add mapping of **hostnames** to **IP addresses** by adding additional lines to the `/etc/hosts` file.

* **Deduction**: Check file `15`.

> Since we need **superuser** privileges to edit the `/etc/hosts` file, in order to execute the command line in file `15` we would need to run: `sudo sh 15`

## System
### Exercise 01
In what file can you find the installed version of your Debian?

* **Explanation**: In `/etc/debian_version`; self-explanatory.

* **Deduction**: Check file `02`.

### Exercise 02
What command can you use to rename your system?

* **Explanation**: In Linux, the `hostnamectl` command may be used to **query** and **change** the system hostname and related settings.

* **Command**: Check file `02`.

### Exercise 03
What file has to be modified to make it permanent?

* **Explanation**: That file is `/etc/hostname` which contains the name of the machine, used by applications that run locally. That name can be retrieved by the `hostname` command, and temporarily changed by the `hostnamectl` command.

* **Deduction**: Check file `03`.

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

### Exercise 14
Run the ‘tail -f /var/log/syslog‘ command in background.

* **Explanation**: In Debian systems, the `/var/log/syslog` file contains a data log of **all activity** that has taken place in the system. For that reason, the file may grow to a considerable size; the `tail` is used to output the last part of a file (by default the last 10 lines). When we use the `-f` option (short for `--follow`), we'll be *following* all the information being appended to the `/var/log/syslog`. Since that would take over our shell session, we could send it to the **background** adding an `&` at the end of the command.

> We need **superuser** privileges to open the `/var/log/syslog` file, so `sudo` needs to be used.

* **Command**: Check file `14`.

> As soon as we send the process to the background, we may still get some output; pressing the **enter** key should make that output dissapear and will bring up our prompt.

### Exercise 15
Find the command that kills the background command’s process.

* **Explanation**: In Unix/Linux systems, the `kill` command is used to **send signals** to processes. The  **default signal** for `kill` is `SIGTERM` or `TERM` (used to stop a background process gracefully). Alternate signals may be specified in three ways: `-9`, `-SIGKILL` or `-KILL` (also `-s KILL`).

> The `kill` command needs the **PID** of the process we want to send a signal. We can get the **PID** of the `tail` processes with `ps ax | grep tail`.

Since we're trying to kill a process running in the **background**, the `jobs` command can be used to get the **job identifier** of such background processes, and use that **job id** to send the signal.

* **Command**: Check file `15`.

### Exercise 16
Find the service which makes it possible to run specific tasks following a regular schedule.

* **Explanation**: In Unix/Linux systems, `cron` is a command-line utility used to schedule jobs  (commands or shell scripts) to run periodically at fixed times, dates, or intervals. Cron is most suitable for scheduling repetitive tasks. Scheduling one-time tasks can be accomplished using the associated `at` utility. 

* **Deduction**: Check file `16`.

### Exercise 17
Find the command that allows you to connect via ssh on the VM. (In parallel with the graphic session)

* **Explanation**: In order to connect via `ssh` to our **Debian Virtual Machine**, we'll have to do some work on the **host system** (in my case I'm running Virtual Box on a macOS host). So from **Virtual Box** we have to click on the virtual machine **Settings > Network > Advanced > Por Forwarding**. Once there we have to click the icon to the right (plus sign) to **add a new port forwarding rule**. In that rule we'll add:

	* Name: whatever you want.
	* Protocol: TCP/IP.
	* Host IP: 127.0.0.1.
	* Host Port: 2222.
	* Guest IP: 10.0.2.15.
	* Guest Port: 22.

Then, we have to:

	* Install the `ufw` firewall in the **Debian Virtual Machine**: `sudo apt install ufw`
	* Enable the **firewall**: `sudo ufw enable`
	* Create a **firewall rule** to allow ssh: `sudo ufw ssh allow`
	* Get the IP address of the **guest**: Run `ip address` (10.0.2.15)

Now, from the **host** (macOS) we can connect to the **Debian Virtual Machine** running:

ssh javi@127.0.0.1 -p 2222

* **Command**: Check file `17`.

### Exercise 18
Find the command that kills ssh service.

* **Explanation**: As we mentioned before, the `ps ax` can be grepped for `sshd` to get the **PID** of the `ssh` daemon (service), and then use the `kill` command with that **PID**. Another option is to use the `pkill` command, which signal processes by name.

> If we want to make sure, we could run first the `pgrep` command to print the process IDs of all processes that match the criteria given as argument.

* **Command**: Check file `18`.

### Exercise 19
List all services which are started at boot time and name this kind of services.

* **Explanation**: Services or [daemons](https://en.wikipedia.org/wiki/Daemon_(computing)) are programs that are usually started when the system boots up, and that run in the background. As we mentioned before, most of Linux based OS have adopted `systemd` to organize and manage their services. In `systemd`, daemons are configured in **unit files**, which are kept in several locations. We could use the `systemctl` command to list all the **units** that are **enabled**, meaning that are started by `systemd` when the system boots.

* **Command**: Check file `19`.

### Exercise 20
List all existing users on the VM.

* **Explanation**: All user accounts are listed in the `/etc/passwd` file, which contains a line per account. We can easily list users using the `cat` command, then grepping the first field, which contains the **user name**.

> Each line in `/ect/passwd` contain seven fields delimited by colons.

Another way to achieve exactly the same as described above, it's to use the `getenv` command, which can be used to get the contents of the `passwd` database.

> The `/etc/passwd` file is a **text-based database** of information about user accounts.

* **Command**: Check file `20`.

### Exercise 21
List all real users on the VM.

* **Explanation**: The output of the last command contains **all users accounts** in the system. But some of these accounts exist to manage **services**, and only some of them belong to real users (people).

> Usually, a **real user account** has a real **login shell** and a **home directory**.

Each user has a numeric user ID called **UID** (3rd field of a `passwd` record). Real users are created with a user ID between `UID_MIN` and `UID_MIN`, which are values set in the `/etc/login.defs` file.

> You can check these values running `cat /etc/login.defs | grep UID`.

So to get all **real users** in the system we could run:
```
getent passwd {1000..6000} | cut -d: -f1
```

The `{1000..6000}` is known as **brace expansion** and it's suported by a lot of shells; it expands to all values between 1000 and 6000 (the `UID_MIN` and `UID_MAX` in my system). These values are the numeric keys that `getent` uses to filter the matches from the `passwd` database. Then we just have to `cut` the first field.

* **Command**: Check file `21`.

### Exercise 22
Find the command that add a new local user.

* **Explanation**: There are a couple of utilities for creating new users in a Linux system:

	* `useradd`, which is a native binary compiled with the system.
	* `adduser`, which is a Perl script which uses useradd binary in back-end.

The second one is **interactive** and friendlier to use, whereas the first one requires a bit of `man` reading.

> Both of these tools require at least a **username** as argument, and also **superuser** privileges to be run, so `sudo` must be used.

* **Command**: Check file `22`.

### Exercise 23
Explain how connect yourself as new user. (With graphic session and ssh session)

* **Explanation**: it's in the deduction file.

* **Deduction**: Check file `23`.

### Exercise 24
Find the command that list all packages.

* **Explanation**: Debian uses [APT](https://en.wikipedia.org/wiki/APT_(software)) as a **package manager**. The question seems to refer to the list of **installed packages**:
```
apt list --installed
```

> **Listing** packages doesn't require **superuser** privileges; **installing/removing/updating** them do require it, so using `sudo` is in order.

* **Command**: Check file `24`.

## Scripting
### Exercise 1
Write a script which displays only the login, UID and Path of each entry of the /etc/passwd file.

* **Explanation**: As we mentioned before, the [passwd](https://en.wikipedia.org/wiki/Passwd#Password_file) database contained in the `/etc/passwd` file, organizes each user account information in **records**. Each record is represented by a line, and each line has **7 fields**. For this exercise the three relevant fields are:

	* **Username** aka **login**, in field `1`.
	* **UID**, in field `3`.
	* **Path to login shell**, in field `7`.

Since the `/etc/passwd` file usually contains **comments** at the top, instead of `cat` its contents, we used the `getent passwd`, so comments are skipped making easier to filter the content.

* **Script**: Check file `01`.

### Exercise 2
Write a script which delete an ACTIVE user on the VM.

* **Explanation**: The assignment seems to ask for the deletion of a user who's currently **logged in** into the system, so the first thing we should do is killing all the processes started by that user; otherwise we'd get:
```
$ sudo userdel bob
userdel: user bob is currently used by process 10711
```
The other big question to consider is checking that the script can't delete the **current user**, otherwise we could leave the system in an inconsistent state, if the user we're trying to delete is the **last user** in the system.

* **Script**: Check file `02`.
