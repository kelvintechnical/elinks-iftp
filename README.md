# Command-Line Web and FTP Testing (RHEL 9)
### RHCSA EX200 Lab | Part of [linux-ops-mastery](https://github.com/kelvintechnical/linux-ops-mastery)
> Use elinks to test web server connectivity from the terminal and lftp to download and upload files to an FTP server.

![RHCSA](https://img.shields.io/badge/RHCSA-EX200-EE0000?style=flat&logo=redhat&logoColor=white)
![Topic](https://img.shields.io/badge/Topic-Remote_Administration-blue)

---

## 📋 Scenario

On **Node1**, use the `elinks` text-based browser to verify web server connectivity from the terminal, and use `lftp` to connect to an FTP server and practice downloading and uploading files.

---

## 🎯 Requirements

1. Install `elinks` and `lftp`.
2. Use `elinks` to fetch a web page from the command line.
3. Use `lftp` to connect to a public FTP server.
4. Practice `get`, `mget`, and `put` commands inside `lftp`.

---

## ✅ Tasks

- Install both tools via `dnf`
- Use `elinks` to test the Apache server running on localhost
- Use `lftp` to connect, list, download, and upload files

---

## 📚 Command Decision Map

| Lab Phrase | Question Being Asked | Tool |
|------------|---------------------|------|
| "Test web server from terminal" | How do I browse without a GUI? | `elinks http://...` |
| "Text-based browser" | What renders HTML in the terminal? | `elinks` |
| "FTP client" | What CLI tool handles FTP? | `lftp` |
| "Download a file" | How do I get one file via FTP? | `get filename` inside lftp |
| "Download multiple files" | How do I get many files at once? | `mget pattern` inside lftp |
| "Upload a file" | How do I send a file via FTP? | `put filename` inside lftp |

---

## 🧠 Big Concept — Why CLI web and FTP tools?

On a production Linux server there is no GUI browser. `elinks` and `curl` are how you verify web services are responding from the server itself — useful for testing Apache configs without leaving the terminal. `lftp` is a full-featured FTP/SFTP client used in scripts and cron jobs where interactive GUI tools aren't available.

---

## Step 1 — Install elinks and lftp

```bash
sudo dnf install elinks lftp -y
```

---

## Step 2 — Test web connectivity with elinks

Use `elinks` to verify your Apache server (from the Apache lab) is serving content:

```bash
elinks http://localhost
```

> `elinks` opens an interactive text browser. Press `q` to quit.

**Non-interactive mode — dump page to terminal:**
```bash
elinks -dump http://localhost
# Expected: RHCSA V9 MOCK EXAM 4 in progress!

elinks -dump http://localhost/web/practice.html
# Expected: Welcome to RHCSA Custom Web Content Test
```

> `-dump` prints the page content directly to stdout — no interactive UI. Useful in scripts.

---

## Step 3 — Create a test file to upload

```bash
echo "lftp upload test file" > /tmp/testfile.txt
```

---

## Step 4 — Connect to a public FTP server with lftp

```bash
lftp ftp.dlptest.com
```

> `ftp.dlptest.com` is a free public FTP test server — no account needed for anonymous access.

**Inside the lftp session:**

```bash
# List files on the server
ls

# Download a single file
get README.txt

# Download multiple files matching a pattern
mget *.txt

# Upload your test file
put /tmp/testfile.txt

# Exit
quit
```

---

## Step 5 — One-liner lftp download (no interactive session)

```bash
lftp -e "get README.txt; quit" ftp.dlptest.com
```

> Useful for scripts — connects, runs the command, and exits automatically.

---

## 🧠 Key Concepts

| Concept | What it means |
|---------|--------------|
| `elinks` | Terminal-based HTML browser — renders web pages without a GUI |
| `elinks -dump URL` | Non-interactive mode — prints page to stdout |
| `lftp` | Feature-rich CLI FTP/SFTP client |
| `get filename` | Download one file from the FTP server |
| `mget pattern` | Download multiple files matching a glob pattern |
| `put filename` | Upload a local file to the FTP server |
| `ls` inside lftp | List files on the remote FTP server |
| `quit` | Exit the lftp session |

---  

## ⚠️ Pitfalls

- **`elinks` opens interactive mode by default** → use `-dump` for scripting or quick terminal checks
- **Public FTP servers go offline** → if `ftp.dlptest.com` is unreachable, try `ftp.gnu.org` as an alternative
- **`mget` without a pattern downloads everything** → always specify a pattern like `*.txt` to avoid pulling large directories
- **`put` requires write permission on the server** → anonymous FTP servers are often read-only; use a test server that allows uploads

---

## ✅ Lab Checklist

- `elinks` and `lftp` installed via `dnf` ✓
- `elinks -dump http://localhost` returns Apache content ✓
- `lftp ftp.dlptest.com` connects successfully ✓
- `ls` lists remote files ✓
- `get` downloads a file ✓
- `mget` downloads multiple files ✓
- `put` uploads a file ✓

---

## 🔗 Related Labs

- [Configure Apache to Serve Default and Custom Web Content](https://github.com/kelvintechnical/apache-custom-content)
- [Network Troubleshooting with telnet and nmap](https://github.com/kelvintechnical/telnet-nmap)
- [Full RHCSA/RHCE Study Guide →](https://github.com/kelvintechnical/linux-ops-mastery)

---

## 👤 Author

**Kelvin R. Tobias** — [kelvinintech.com](https://kelvinintech.com) | [GitHub](https://github.com/kelvintechnical) | [LinkedIn](https://www.linkedin.com/in/kelvin-r-tobias-211949219)
