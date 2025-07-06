# 🐧 Day 13 – Networking

## Table of Contents
1. [ping – Check if a computer is online](#1-ping-–-check-if-a-computer-is-online)  
2. [traceroute – See the path packets take](#2-traceroute-–-see-the-path-packets-take)  
3. [dig – Look up website addresses](#3-dig-–-look-up-website-addresses)  
4. [curl – Download or talk to a website](#4-curl-–-download-or-talk-to-a-website)  
5. [ip – View and set your computer’s network address](#5-ip-–-view-and-set-your-computers-network-address)  
6. [ss – See your computer’s connections](#6-ss-–-see-your-computers-connections)  
7. [tcpdump – Capture live network traffic](#7-tcpdump-–-capture-live-network-traffic)  
8. [netcat (nc) – Talk on open ports](#8-netcat-nc-–-talk-on-open-ports)  
9. [nmap – Scan a network for computers](#9-nmap-–-scan-a-network-for-computers)  
10. [iftop – Watch network speed live](#10-iftop-–-watch-network-speed-live)  
11. [Quick Practice Examples](#11-quick-practice-examples)

---

<details>
<summary><strong>1. ping – Check if a computer is online</strong></summary>

**Why use it?** To see if another computer (or website) can talk back.

- **What it does**: Sends a small message called an ICMP echo request. If the other computer is on and reachable, it sends the same message back.
- **Basic use**:
  ```bash
  ping example.com


This keeps sending messages until you stop it (Ctrl+C).

* **Count option**: Send only a few messages.

  ```bash
  ping -c 3 example.com
  ```

  * `-c 3` means “stop after 3 messages.”

* **What you’ll see**:

  ```text
  PING example.com (93.184.216.34): 56 data bytes
  64 bytes from 93.184.216.34: icmp_seq=0 ttl=56 time=10.2 ms
  64 bytes from 93.184.216.34: icmp_seq=1 ttl=56 time=10.5 ms
  64 bytes from 93.184.216.34: icmp_seq=2 ttl=56 time=10.1 ms
  --- example.com ping statistics ---
  3 packets transmitted, 3 received, 0% packet loss
  round-trip min/avg/max = 10.1/10.3/10.5 ms
  ```

  * **`time=10.2 ms`** tells you how fast (lower is better).

</details>

<details>
<summary><strong>2. traceroute – See the path packets take</strong></summary>

**Why use it?** To find where network delays happen between you and another server.

* **What it does**: Sends test messages with increasing “time to live” (TTL). Each router along the way shows where it passed through and how long each step took.

* **Basic use**:

  ```bash
  traceroute example.com
  ```

* **Skip name lookups** (faster output):

  ```bash
  traceroute -n example.com
  ```

  * `-n` shows only IP addresses without trying to turn them into names.

* **What you’ll see**:

  ```text
   1  192.168.1.1   1.123 ms  0.987 ms  1.045 ms
   2  10.0.0.1     10.234 ms 10.456 ms 10.112 ms
   3  93.184.216.34 20.333 ms 20.221 ms 20.412 ms
  ```

  * Each numbered line is one “hop” (router).
  * The times are how long each hop took.

</details>

<details>
<summary><strong>3. dig – Look up website addresses</strong></summary>

**Why use it?** To see the IP address (and other info) behind a website name.

* **What it does**: Asks DNS servers “What IP is example.com?”

* **Basic use**:

  ```bash
  dig example.com
  ```

* **Short answer only**:

  ```bash
  dig +short example.com
  ```

  * `+short` means “just show me the IP(s).”

* **What you’ll see**:

  ```text
  93.184.216.34
  ```

</details>

<details>
<summary><strong>4. curl – Download or talk to a website</strong></summary>

**Why use it?** To grab a page or talk to a web service without a browser.

* **What it does**: Sends HTTP or HTTPS requests and shows you the response.
* **Basic use** (download a page):

  ```bash
  curl http://example.com
  ```
* **See headers only**:

  ```bash
  curl -I http://example.com
  ```

  * `-I` means “show only the response headers, not the page body.”
* **Save output to a file**:

  ```bash
  curl http://example.com -o page.html
  ```

  * `-o page.html` writes the response into `page.html`.

</details>

<details>
<summary><strong>5. ip – View and set your computer’s network address</strong></summary>

**Why use it?** To check or change your computer’s IP address and network interfaces.

* **What it does**: Replaces older tools like `ifconfig` with more details.
* **Show your IP addresses**:

  ```bash
  ip addr show
  ```
* **Bring an interface up** (turn it on):

  ```bash
  sudo ip link set eth0 up
  ```

  * `eth0` is the interface name (yours might be `enp3s0` or `wlan0`).
* **Add a new IP**:

  ```bash
  sudo ip addr add 192.168.1.50/24 dev eth0
  ```

  * Sets your computer’s address to `192.168.1.50` on a 255.255.255.0 network.

</details>

<details>
<summary><strong>6. ss – See your computer’s connections</strong></summary>

**Why use it?** To list which programs are talking to the network.

* **What it does**: Shows active TCP/UDP sockets (connections).
* **Show all TCP connections**:

  ```bash
  ss -t
  ```
* **Show listening ports only**:

  ```bash
  ss -l
  ```

  * `-l` means “listening” (waiting for connections).
* **Full view (no name lookups)**:

  ```bash
  ss -tunp
  ```

  * `-t` TCP, `-u` UDP, `-n` numeric only, `-p` show process name.

</details>

<details>
<summary><strong>7. tcpdump – Capture live network traffic</strong></summary>

**Why use it?** To record exactly what goes in and out of your network interface.

* **What it does**: Saves raw packets so you can inspect them.
* **Basic capture**:

  ```bash
  sudo tcpdump -i eth0 -c 5 -nn
  ```

  * `-i eth0` choose interface, `-c 5` stop after 5 packets, `-nn` no name lookups.
* **Save to a file**:

  ```bash
  sudo tcpdump -i eth0 -w capture.pcap
  ```

  * `-w capture.pcap` writes packets to `capture.pcap` for later analysis.

</details>

<details>
<summary><strong>8. netcat (nc) – Talk on open ports</strong></summary>

**Why use it?** To send or receive raw data over TCP or UDP, often for testing.

* **What it does**: Opens a simple connection to a port.
* **Check if port 80 is open**:

  ```bash
  nc -vz example.com 80
  ```

  * `-v` verbose, `-z` zero-I/O (just test connect).
* **Listen on a port** (simple server):

  ```bash
  nc -l -p 1234 > received.txt
  ```

  * Waits on port 1234 and writes incoming data to `received.txt`.

</details>

<details>
<summary><strong>9. nmap – Scan a network for computers</strong></summary>

**Why use it?** To find which computers and services are available on a network.

* **What it does**: Probes a range of IPs and ports.
* **Scan a single host**:

  ```bash
  nmap example.com
  ```
* **Scan a subnet**:

  ```bash
  nmap 192.168.1.0/24
  ```
* **Fast scan specific ports**:

  ```bash
  nmap -p 22,80,443 example.com
  ```

</details>

<details>
<summary><strong>10. iftop – Watch network speed live</strong></summary>

**Why use it?** To see which connections use the most bandwidth right now.

* **What it does**: Shows a real-time table of data rates per connection.
* **Run on interface**:

  ```bash
  sudo iftop -i eth0
  ```
* **Show only IPs** (no DNS lookups):

  ```bash
  sudo iftop -n -i eth0
  ```

</details>

<details>
<summary><strong>11. Quick Practice Examples</strong></summary>

Try these in your terminal:

1. Check if Google is online and stop after 2 pings:

   ```bash
   ping -c 2 google.com
   ```
2. Find how many hops to your router:

   ```bash
   traceroute -n 192.168.1.1
   ```
3. See your own IP address:

   ```bash
   ip addr show
   ```
4. Download example.com homepage into a file:

   ```bash
   curl http://example.com -o homepage.html
   ```
5. List listening TCP ports:

   ```bash
   ss -ltnp
   ```

</details>