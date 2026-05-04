# HumaniseIPV4
A function showcase of how IPV4 ips and IPV4+Port can be written in more human manners, allowing their use in apps where the user needs to enter the hosters IP to connect. This allows making the GUI more user-friendly.
Below are two **human‑friendly** functions for converting an IPv4 address (and optionally a port) into short, case‑sensitive alphanumeric codes, along with their reverse functions.  

The idea is to give users a **memorable, easy‑to‑type identifier** instead of a raw IP address – for example, when connecting to a game server, a remote SSH host, or any service where typing a numeric IP is cumbersome.

## How it works
- An IPv4 address is turned into a 32‑bit integer.
- For **IP+port**, the port (16 bits) is appended to the IP (32 bits) to form a 48‑bit integer.
- That integer is encoded using **base‑62** (digits `0‑9`, `A‑Z`, `a‑z`), producing a short string.
- The reverse functions decode the string back to the original IP and port.

### Why base‑62?
- 62 characters cover the same key space as lower/upper case letters and digits.
- The resulting codes are **case‑sensitive**, which doubles the density compared to base‑36.
- For an IPv4 address alone, **6 characters** are enough (since 62⁶ > 2³²).
- For IP+port, the code length varies from 1 to 9 characters, **using the minimum possible** for each combination.

## Use case examples
- **User‑friendly connection strings**  
  Instead of asking a friend to type `192.168.1.100:25565` to join your Minecraft server, you give them `aB3xR7`.  
  The code is short enough to read over voice chat or write on a sticky note.

- **URL shortening for local resources**  
  A web dashboard could show `https://connect/aB3xR7` instead of a long IP.

- **Command line shortcuts**  
  `ssh $(dehumanise_ip aB3xR7)` – combine with shell aliases.

### Example output (actual results may vary because of different base‑62 mapping to the same integer)
```
192.168.1.100  ->  E4GZvW  ->  192.168.1.100
10.0.0.1:8080  ->  1vXoNQ  ->  10.0.0.1:8080
```

## Important notes
- The functions are **pure** – they do no I/O, only conversion.
- For `humanise_ip_port`, the code is **minimal** (no leading `'0'`). A combined value that happens to start with zero digits in base‑62 will produce a shorter code.
- The reverse functions accept **any** valid base‑62 string of the appropriate length (exactly 6 for IP, variable for IP+port). Invalid characters raise a `KeyError`.
- If you need a **fixed length** for IP+port as well, simply pad the result of `humanise_ip_port` with leading `'0'` up to 9 characters, but that would waste the “minimum” requirement.
