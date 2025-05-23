
# 📁 nefser_file_transfer

**Nefser** is a command-line tool for fast and simple file transfer over a network using a single command.

---

## 🚀 How to Use

```bash
python3 nefser.py [argument] [value]
python3 nefser.py --help       # View the help menu
```

---

## ⚙️ Options

| Option               | Description                                             |
|----------------------|---------------------------------------------------------|
| `--help`             | Show the help menu                                      |
| `-recv [file/path]`  | Start a TCP server to receive a file                    |
| `-send [file/path]`  | Start a TCP client to send a file                       |
| `-i [IP/FQDN]`       | Set the IP address or FQDN for sending/receiving data  |
| `-p [port]`          | Set the network port to use                             |
| `-t [sec]`           | Set the listening time (in seconds) for receiving data |

---

## 📝 Default Behavior

If no arguments are provided, the default command is:

```bash
python3 nefser.py -recv nefser.bytes -i 0.0.0.0 -p 8080 -t 10
```

---

## 🔒 Notes

- Make sure the receiving side is already listening before sending the file.
- Works on LAN and over the internet if port forwarding is properly configured.

---
## 📡 How it works

This script follows a 6-step process to send or receive data reliably between a **Sender** and a **Receiver**.

### 📨 Sender Workflow
1. **Establish connection** — Perform a three-way handshake.  
2. **Send file length** — Transmit 5 bytes indicating the length of the file.  
3. **Send hash** — Transmit 33 bytes consisting of the MD5 hash of the file followed by a newline character (`\n`).  
4. **Send file** — Send the actual file data.  
5. **Validate (optional)** — Wait for hash validation response from the receiver (if applicable).  
6. **Close connection** — Gracefully close the connection.

### 📥 Receiver Workflow
1. **Establish connection** — Perform a three-way handshake.  
2. **Receive file length** — Read 5 bytes indicating the length of the file.  
3. **Receive hash** — Read 33 bytes containing the MD5 hash and newline character (`\n`).  
4. **Receive file** — Receive the actual file data.  
5. **Validate (optional)** — Send hash validation result back to the sender (if applicable).  
6. **Close connection** — Gracefully close the connection.

---
Made by NeannGH
