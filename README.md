# 🔓 Password Hash Extraction & Cracking Lab

**Hands-on practice with hash extraction and dictionary attacks against my own test files** 🧪🔑

**By SasTech** 🚀

![John the Ripper](https://img.shields.io/badge/John%20the%20Ripper-Password%20Cracking-black?logo=hackaday&logoColor=white)
![Kali](https://img.shields.io/badge/Kali-Linux-557C94?logo=kalilinux&logoColor=white)
![PDF](https://img.shields.io/badge/File%20Type-PDF%20%26%20ZIP-DC382D)
![Networkwalks](https://img.shields.io/badge/Networkwalks-Academy-2D2D2D)
![Status](https://img.shields.io/badge/Status-Week%203%20Completed-brightgreen)
![SasTech](https://img.shields.io/badge/By-SasTech-orange)

---

## ⚠️ Disclaimer

I performed every activity in this repository only against **files I created and own myself**, as part of Week 3 of my Cybersecurity internship with Networkwalks Academy. All tools and techniques here are for **education and authorized testing only**.

Password cracking against any file, account or system you do not own, or do not have explicit written permission to test, is illegal in most countries — even when no real damage is caused. This repository is a learning log, not an invitation to attack anyone else's data. The author, Networkwalks, and anyone else referenced here are not responsible for misuse of this information.

---

## 📌 Overview

This repository documents what I learned in Week 3 of my internship about **how password hashes work** and **how dictionary attacks crack them**. Over the week I:

- Learned what a password hash is, and why a hash extracted from a protected file (PDF, ZIP, etc.) is what gets attacked, not the file itself.
- Learned how John the Ripper (JtR) and similar dictionary-attack tools compare a wordlist, hashed one guess at a time, against the target hash until one matches.
- Learned how to build and choose a wordlist (password list) for a dictionary attack.
- Extracted hashes from and successfully cracked the passwords of **5–6 protected files** (PDF and ZIP), using both command-line and browser-based tools.

The goal of this repo is to document the process clearly enough that someone new to the topic can follow the same steps on their **own** test files.

---

## 🧰 Tools Used

| Tool | Type | Purpose |
|---|---|---|
| [PDF Hash Extractor – OnlineHashCrack](https://www.onlinehashcrack.com/tools-pdf-hash-extractor.php) | Web tool | Extracts the crackable `$pdf$...` hash from a password-protected PDF |
| [Hash Calculator – Networkwalks Academy](https://networkwalks.com/hash-calculator/) | Web tool | Generates/inspects hashes for lab files |
| [Password Cracker (Dictionary Attack) – Networkwalks Academy](https://networkwalks.com/password-cracker/) | Web tool | Browser-based simulation of a dictionary attack against a hash, using a wordlist |
| John the Ripper (JtR) | CLI (Kali Linux) | Industry-standard offline password cracker; ran the same dictionary-attack logic locally |
| `zip2john` / `pdf2john` | CLI (Kali Linux, JtR suite) | Converts a protected ZIP/PDF into a hash format JtR can crack |
| Custom wordlists | Text file | Self-built password lists used as the dictionary source |

---

## 🧠 Key Concepts Learned

### 1. What a hash is
A hash is a fixed-length, one-way "fingerprint" of data (in this case, the password needed to open a file). The actual password is never stored in the file — only its hash. You cannot reverse a hash back into the password directly; you can only **guess** passwords, hash each guess, and compare it to the stored hash.

### 2. How a dictionary attack works
1. Extract the file's hash (e.g. `$pdf$4*4*128*...` for a PDF, or JtR's `$zip2$...` format for a ZIP).
2. Load a wordlist — a text file of candidate passwords (common passwords, names, patterns, etc.).
3. The tool hashes each word in the list using the same algorithm/parameters as the original file.
4. Each resulting hash is compared to the target hash.
5. A match reveals the plaintext password.

This is exactly what both the Networkwalks **Password Cracker** tool and **John the Ripper** do — the website version shows it visually, JtR does it at real speed on the command line.

### 3. Why wordlist quality matters
A dictionary attack can only find a password that is actually *in* the wordlist. Short, generic lists work well for weak/common passwords (e.g. `password1`, `1qaz2wsx`). Real-world wordlists (like `rockyou.txt`) succeed because people reuse predictable passwords. Stronger, random passwords defeat dictionary attacks entirely — which is itself one of the key lessons of this exercise.

---

## 🪜 Step-by-Step: How I Extracted & Cracked Passwords

### A. PDF files (browser method)

1. Go to the [PDF Hash Extractor](https://www.onlinehashcrack.com/tools-pdf-hash-extractor.php) and upload the password-protected PDF.
2. Copy the extracted hash (format: `$pdf$4*4*128*...`).
3. Open the Networkwalks [Hash Calculator](https://networkwalks.com/hash-calculator/) and [Password Cracker](https://networkwalks.com/password-cracker/) tool.
4. Paste the `$pdf$...` hash into the cracker.
5. Run the attack — the tool hashes each candidate from its built-in wordlist and checks it against the pasted hash.
6. On a match, the tool displays the cracked password (e.g. `password1`, `1qaz2wsx`).
7. Open the original PDF and confirm it unlocks with that password.

### B. PDF / ZIP files (command-line method, John the Ripper)

```bash
# 1. Convert the protected file into a crackable hash format
pdf2john.pl protected.pdf > pdf_hash.txt
zip2john protected.zip > zip_hash.txt

# 2. Run John the Ripper with a wordlist against the hash
john --wordlist=my_wordlist.txt pdf_hash.txt
john --wordlist=my_wordlist.txt zip_hash.txt

# 3. Show the cracked password once found
john --show pdf_hash.txt
john --show zip_hash.txt
```

### C. Building a wordlist

- Started from common/default password lists (e.g. `rockyou.txt`, provided with Kali Linux).
- Added likely candidates relevant to the lab scenario (short, simple, predictable passwords).
- Saved as a plain `.txt` file, one password per line — the format both JtR and the Networkwalks tool expect.

---

## 🏁 Results

| # | File type | Method used | Result |
|---|---|---|---|
| 1 | PDF | OnlineHashCrack extractor + Networkwalks Password Cracker | ✅ Cracked — password `password1` |
| 2 | PDF | OnlineHashCrack extractor + Networkwalks Password Cracker | ✅ Cracked — password `1qaz2wsx` |
| 3 | PDF | `pdf2john` + John the Ripper | ✅ Cracked |
| 4 | ZIP | `zip2john` + John the Ripper | ✅ Cracked |
| 5–6 | PDF / ZIP | Mixed (web tool + JtR) | ✅ Cracked |

See `screenshots/` for evidence of each successful crack, including flags captured on the Networkwalks Academy lab platform.

---

## 📸 Evidence

| Screenshot | Description |
|---|---|
| `01-flag-captured-1.png` | Flag captured after successfully cracking a file password |
| `02-flag-captured-2.png` | Flag captured on the John the Ripper / persistence lab module |
| `03-dictionary-attack-success-1.png` | Networkwalks Password Cracker tool — dictionary attack matched `password1` against the extracted PDF hash |
| `04-dictionary-attack-success-2.png` | Networkwalks Password Cracker tool — dictionary attack matched `1qaz2wsx` against a second extracted PDF hash |
| `05-flag-captured-3.png` | First flag captured on the Networkwalks Academy lab platform |

---

## 💡 Key Takeaways

- A hash extracted from a file, not the file itself, is what a cracking tool attacks.
- Dictionary attacks are only as strong as the wordlist behind them.
- Common/reused passwords (`password1`, `1qaz2wsx`, etc.) fall almost instantly to a basic wordlist.
- Strong, unique, randomly generated passwords are the actual defense against this technique — not "security through obscurity."
- Always test only on files, accounts, or systems you own or are explicitly authorized to test.

---

## 📂 Repository Structure

```
.
├── README.md
└── screenshots/
    ├── 01-flag-captured-1.png
    ├── 02-flag-captured-2.png
    ├── 03-dictionary-attack-success-1.png
    ├── 04-dictionary-attack-success-2.png
    └── 05-flag-captured-3.png
```

---

## 🔗 Related

- [Week 2 Report: Footprinting & Network Scanning](https://github.com/sas-tech705/WEEK_2-PENTESTIN-TESTING-REPORT)





## 👤 Author

**ABDUL SAMED SEIDU**
Cybersecurity Professional

🔗 LinkedIn: [linkedin.com/in/abdul-samed-seidu-44678635a](https://www.linkedin.com/in/abdul-samed-seidu-44678635a/)

