# 🔐 Hashcat — Advanced Password Recovery & Hash Auditing | Cybersecurity Learning Notes

Welcome to my **Hashcat Learning Repository!**

As part of my cybersecurity learning journey, I explored Hashcat, an advanced open-source password recovery and security auditing tool.

Hashcat helps security professionals evaluate password security by testing password candidates against stored hashes in authorized environments.

This repository documents my learning, starting with the introduction to Hashcat and its purpose, followed by its working mechanism, attack modes, supported hash algorithms, CPU and GPU processing, advantages, limitations, and real-world applications.

I have also included **six educational posters** to make the concepts easier to understand through visual explanations.

The objective of this repository is not simply to memorize commands, but to understand how password auditing works, how different attack strategies operate, and how security professionals use these techniques to identify weak passwords.

> **Ethical use:** Hashcat must only be used with password hashes and systems that you own or have explicit permission to audit.

---

# 📚 Table of Contents

1. [Introduction to Hashcat](#1-introduction-to-hashcat)
2. [Why Do We Use Hashcat?](#2-why-do-we-use-hashcat)
3. [How Does Hashcat Work?](#3-how-does-hashcat-work)
4. [Understanding Password Candidates and Hashes](#4-understanding-password-candidates-and-hashes)
5. [Hashcat Attack Modes](#5-hashcat-attack-modes)
6. [Supported Hash Algorithms and Formats](#6-supported-hash-algorithms-and-formats)
7. [Understanding CPU vs GPU](#7-understanding-cpu-vs-gpu)
8. [Hashcat vs John the Ripper](#8-hashcat-vs-john-the-ripper)
9. [Important Hashcat Commands and Options](#9-important-hashcat-commands-and-options)
10. [Advantages and Key Features](#10-advantages-and-key-features)
11. [Limitations of Hashcat](#11-limitations-of-hashcat)
12. [Real-World Applications](#12-real-world-applications)
13. [Hashcat Learning and Practice Roadmap](#13-hashcat-learning-and-practice-roadmap)
14. [Important Security Defenses](#14-important-security-defenses)
15. [Final Key Takeaways](#15-final-key-takeaways)
16. [Official Learning Resources](#16-official-learning-resources)

---

# 1. Introduction to Hashcat

**What is Hashcat?**

Hashcat is a powerful, open-source password recovery and password auditing tool used to test password candidates against stored password hashes.

It is designed to use available computing hardware, including CPUs and GPUs, to perform password recovery operations.

Hashcat supports multiple password-hashing algorithms, attack strategies, and hardware acceleration technologies.

### In simple words

Imagine a system stores a password as a hash instead of saving the original password.

For example:

- Original password: `hello123`
- Stored hash: A hash value generated from `hello123`.

If a security auditor has an authorized copy of that hash, Hashcat can test possible password candidates and check whether one produces a matching result.

If a match is found, the candidate password has been identified.

### Important characteristics

- Open-source password recovery software.
- Supports CPU and GPU processing.
- Provides multiple password candidate generation strategies.
- Supports hundreds of hash formats and modes.
- Can process multiple hashes.
- Supports Windows, Linux, and macOS.
- Useful for password security auditing and authorized recovery.

### Is Hashcat an online or offline attack tool?

Hashcat is primarily used for offline password auditing.

It works with obtained hash data and performs candidate testing locally rather than submitting every guess to a website's login form.

It does not need to contact the original website to test candidates against an available hash.

### 📸 Poster 1 — What Is Hashcat?

![What is Hashcat](1.jfif)

---

# 2. Why Do We Use Hashcat?

Hashcat is used to evaluate password strength, identify weak passwords, and support authorized password recovery.

### 1. Recover Lost Passwords

If an authorized user has lost a password but has access to its corresponding hash or recovery data, Hashcat may help identify the original password by testing candidates.

### 2. Test Password Strength

Security professionals can use Hashcat to evaluate whether passwords are vulnerable to common guessing strategies.

For example, a password such as `password123` may be found quickly because it follows a common pattern.

### 3. Penetration Testing

During an authorized penetration test, security professionals may assess password hashes to identify weak password choices and recommend improvements.

### 4. Analyze Breached Data

During authorized digital forensics or incident response, security teams may analyze exposed password-hash records to assess password exposure and security risks.

The purpose is to understand the impact and improve protection, not to misuse recovered credentials.

### 5. Learn and Research

Students and researchers can use Hashcat to understand:

- Password hashing.
- Hash algorithms.
- Password candidate generation.
- GPU acceleration.
- Password security weaknesses.
- The effectiveness of different password-storage methods.

### In short

Hashcat helps security professionals test, recover, and evaluate passwords so that weak password practices can be identified and improved.

### 📸 Poster 2 — Why Do We Use Hashcat?

![Why Do We Use Hashcat](2.jfif)

---

# 3. How Does Hashcat Work?

Hashcat works by generating password candidates and comparing their hash results with an available target hash.

It does not reverse the hashing process or decrypt a hash.

### Understanding the working process

**Step 1: Obtain an authorized hash**

A security auditor obtains password-hash data from an authorized test system.

Example:

`5f4dcc3b5aa765d61d8327deb882cf99`

This is an example of an MD5 hash corresponding to `password`.

**Step 2: Identify the hash format**

Hashcat needs to know which hash format it is testing.

The `-m` option specifies the hash mode.

For example:

- `-m 0` — MD5.
- `-m 1000` — NTLM.
- `-m 1400` — SHA-256.
- `-m 3200` — bcrypt.

These are examples of Hashcat mode identifiers, not attack modes.

**Step 3: Select an attack strategy**

The `-a` option selects how Hashcat generates password candidates.

For example:

- Dictionary attack.
- Combination attack.
- Brute-force attack.
- Mask attack.
- Hybrid attack.

**Step 4: Generate password candidates**

Hashcat generates candidates according to the selected strategy.

For example, a wordlist may contain:

- password
- welcome
- hello123
- admin123

**Step 5: Compare the results**

Hashcat processes the candidates using the appropriate hash format and compares the results with the target hash.

**Step 6: Identify a match**

If a candidate produces the matching hash result, Hashcat reports the recovered candidate.

If no candidate matches, the password has not been recovered by that particular attempt.

### Simple working flow

Authorized hash → Identify hash format → Select attack mode → Generate candidates → Compare → Match or no match.

### Important distinction

Hashcat does not guarantee that every password can be recovered.

Success depends on factors such as password strength, candidate coverage, hash algorithm, hardware, and available time.

### 📸 Poster 3 — How Does Hashcat Work?

![How Does Hashcat Work](3.jfif)

---

# 4. Understanding Password Candidates and Hashes

Before learning the attack modes, it is important to understand two terms.

### What is a password candidate?

A password candidate is a possible password that a tool tests against a stored hash.

For example:

- welcome123
- admin123
- hello2026

Each candidate is tested to determine whether it matches the target hash.

### What is a wordlist?

A wordlist is a text file containing possible password candidates.

For example, a file called `passwords.txt` may contain:

```text
welcome
password123
admin123
hello2026
```

Hashcat can use a wordlist as an input source for candidate generation.

### What is a hash mode?

A hash mode identifies the particular hash format Hashcat should process.

Examples:

| Hash mode | Format |
|---|---|
| 0 | MD5 |
| 100 | SHA-1 |
| 1400 | SHA-256 |
| 1700 | SHA-512 |
| 1000 | NTLM |
| 3200 | bcrypt |
| 8900 | scrypt |
| 10900 | PBKDF2-HMAC-SHA256 |

The correct mode depends on the actual hash format and its parameters.

**Remember:**

- `-m` selects the hash format.
- `-a` selects the attack strategy.

These two options perform different jobs.

---

# 5. Hashcat Attack Modes

Attack modes determine how Hashcat generates possible password candidates.

Hashcat provides ten documented attack modes in its current documentation.

Below is a breakdown of their names, mode numbers, and purposes.

## 5.1 Straight Attack — Mode 0

Also commonly called a dictionary or wordlist attack.

This mode takes words from a wordlist and tests them as password candidates.

For example, a wordlist may contain common password choices.

**Why use it?**

It is useful when passwords may have been selected from common words or known password lists.

## 5.2 Combination Attack — Mode 1

This mode combines words from two different wordlists.

For example:

- First wordlist: `blue`, `green`
- Second wordlist: `123`, `2026`

Possible combinations include `blue123` and `green2026`.

**Why use it?**

It helps test passwords that may consist of two combined words or patterns.

## 5.3 Brute-Force Attack — Mode 3

Brute-force testing generates combinations from a defined character set.

For example, if the allowed characters are lowercase letters and numbers, it tests possible combinations from those characters.

**Why use it?**

It is useful when the password structure or length is known or limited.

**Limitation:** The number of possible combinations grows extremely quickly as password length and character variety increase.

## 5.4 PCFG Attack — Mode 4

PCFG stands for Probabilistic Context-Free Grammar.

It uses a trained password-pattern model to generate likely password structures.

Instead of testing every possible random combination, it prioritizes patterns that are more likely to occur based on the model.

**Why use it?**

Human-created passwords often follow recognizable patterns, such as words followed by numbers or symbols.

PCFG attempts to use those patterns intelligently.

## 5.5 Table Attack — Mode 5

Table attacks use predefined tables that specify possible transformations or replacements for password tokens.

For example, a table may define common character substitutions or case transformations.

**Why use it?**

It helps generate alternative forms of words according to predefined transformation tables.

## 5.6 Hybrid Wordlist + Mask — Mode 6

This mode combines a wordlist with a mask appended to the words.

A mask represents a defined character pattern.

For example, a word may be combined with a number pattern.

**Why use it?**

It is useful for testing passwords that contain a recognizable word followed by predictable characters.

## 5.7 Hybrid Mask + Wordlist — Mode 7

This mode reverses the order of mode 6.

The mask-generated characters come first, followed by a word from a wordlist.

**Why use it?**

It helps test passwords where predictable characters appear before a recognizable word.

## 5.8 Generic Attack — Mode 8

This mode uses external feed plugins to generate password candidates.

A feed plugin can provide candidates to Hashcat through a defined interface.

**Why use it?**

It allows more flexible candidate generation and integration with external generators.

## 5.9 Association Attack — Mode 9

Association attacks use contextual information associated with individual hashes to generate candidate passwords.

For example, an authorized password audit may have account-related information available in the input data.

**Why use it?**

It can prioritize candidates based on information connected to the particular hash or account.

## 5.10 Hybrid Attack — Mode 12

This mode uses masks to position words from wordlists within a generated password pattern.

It supports special word markers to place words into specific positions.

**Why use it?**

It provides flexibility when testing passwords that combine known words with other predictable character patterns.

### Attack-mode comparison

| Mode | Name | Main idea |
|---|---|---|
| 0 | Straight | Test words from a wordlist |
| 1 | Combination | Join words from two lists |
| 3 | Brute-force | Generate character combinations |
| 4 | PCFG | Generate likely learned patterns |
| 5 | Table | Apply predefined token transformations |
| 6 | Hybrid Wordlist + Mask | Word followed by a mask |
| 7 | Hybrid Mask + Wordlist | Mask followed by a word |
| 8 | Generic | Use candidate feed plugins |
| 9 | Association | Use hash-associated context |
| 12 | Hybrid | Position words within a mask |

**Important:** Attack mode numbers are not sequential. Mode 2 is not one of the current documented modes in this list.

Also, attack modes describe how candidates are generated. They are not the same thing as hash algorithms.

---

# 6. Supported Hash Algorithms and Formats

Hashcat supports a large range of hash algorithms and application-specific password formats.

The exact supported formats depend on the Hashcat version and its available hash modes.

Here are important examples.

## 6.1 MD5

MD5 produces a 128-bit hash value.

It is fast, but it is no longer considered secure against collisions and is unsuitable for modern password storage.

Hashcat mode:

`-m 0`

## 6.2 SHA-1

SHA-1 produces a 160-bit hash value.

It is deprecated for security-sensitive uses because of its collision weaknesses.

Hashcat mode:

`-m 100`

## 6.3 SHA-2 Family

The SHA-2 family includes:

- SHA-224
- SHA-256
- SHA-384
- SHA-512

Examples of Hashcat modes:

- `-m 1400` — SHA-256.
- `-m 1700` — SHA-512.

SHA-2 algorithms are widely used for integrity checks and other cryptographic purposes. Fast SHA-2 hashes alone are not suitable for password storage.

## 6.4 SHA-3 Family

SHA-3 is a family of cryptographic hash functions based on the Keccak design.

Examples include:

- SHA3-224
- SHA3-256
- SHA3-384
- SHA3-512

Hashcat also supports Keccak-related formats. SHA-3 and Keccak should not automatically be treated as identical formats.

## 6.5 NTLM

NTLM is a password-hashing format associated with Microsoft Windows authentication.

Hashcat mode:

`-m 1000`

## 6.6 bcrypt

bcrypt is a password-hashing algorithm designed to make password guessing more computationally expensive.

Hashcat mode:

`-m 3200`

## 6.7 scrypt

scrypt is a password-based key derivation function that uses significant memory resources.

Hashcat mode:

`-m 8900`

## 6.8 PBKDF2

PBKDF2 is a password-based key derivation function that performs repeated cryptographic operations.

One example is:

`-m 10900` — PBKDF2-HMAC-SHA256.

Other PBKDF2 formats use different mode identifiers.

## 6.9 Other Supported Formats

Hashcat also supports formats associated with:

- WPA/WPA2 authentication data.
- Kerberos authentication.
- MySQL.
- PostgreSQL.
- Microsoft SQL Server.
- Cisco password formats.
- LDAP password formats.
- Various Linux and Unix password-storage formats.

### Important distinction

An algorithm and a hash mode are not the same thing.

For example, SHA-256 is an algorithm, while Hashcat mode 1400 represents a particular raw SHA-256 format.

A salted, iterated, or application-specific SHA-256 construction may require a different mode.

### How to find supported modes

Hashcat provides a command to list its supported hash modes:

```bash
hashcat -hh
```

For a more complete list of command options and supported formats, consult the documentation that matches your installed version.

---

# 7. Understanding CPU vs GPU

One of Hashcat's major features is its ability to use different hardware devices for password recovery operations.

The two important processing units are CPUs and GPUs.

## 7.1 What is a CPU?

CPU stands for Central Processing Unit.

It is the main processor of a computer and handles general-purpose tasks.

### CPU characteristics

- Designed for general-purpose computing.
- Handles complex sequential operations effectively.
- Usually has fewer powerful processing cores than a GPU has smaller processing units.
- Can run Hashcat when supported compute drivers and runtimes are available.

## 7.2 What is a GPU?

GPU stands for Graphics Processing Unit.

It was originally designed to process graphics, but it is also useful for tasks involving many parallel calculations.

Hashcat can use GPU hardware to test many password candidates simultaneously.

### GPU characteristics

- Contains many processing units.
- Designed for parallel processing.
- Can provide high performance for suitable hash algorithms.
- Often performs much faster than a CPU for fast hash formats.
- Requires compatible drivers and compute support.

## 7.3 Why is GPU acceleration useful?

Imagine testing a large number of password candidates.

A CPU may process a smaller number of operations at a time, while a suitable GPU can perform many similar operations in parallel.

This can make a significant difference for some hash formats.

However, performance depends on:

- The hash algorithm.
- Password-hashing parameters.
- GPU architecture.
- CPU capabilities.
- Available memory.
- Drivers and software configuration.
- Thermal and power limits.

## 7.4 CPU vs GPU comparison

| Feature | CPU | GPU |
|---|---|---|
| Full name | Central Processing Unit | Graphics Processing Unit |
| Design | General-purpose processing | Parallel processing |
| Processing cores | Generally fewer, more complex cores | Many smaller processing units |
| Parallel calculations | More limited | Often excellent |
| Hashcat support | Yes | Yes |
| Performance | Can be suitable for many formats | Often much faster for suitable workloads |
| Power and heat | Depends on processor | Can require substantial power and cooling |

## 7.5 Does a GPU always win?

No.

Memory-hard password hashing algorithms such as Argon2id, scrypt, and certain configurations of bcrypt are designed to make password guessing more expensive.

They can reduce the advantage of highly parallel hardware.

The best hardware depends on the algorithm and its settings.

**Remember:** GPU acceleration improves the speed of testing candidates; it does not make a weak password-storage algorithm secure.

---

# 8. Hashcat vs John the Ripper

John the Ripper and Hashcat are both widely used password auditing and recovery tools.

Both can perform offline password-hash testing, but they have different strengths and workflows.

## 8.1 What is John the Ripper?

John the Ripper is an open-source password security auditing tool designed to detect weak passwords.

It supports numerous password formats and provides several cracking strategies.

It is particularly well known for its password auditing workflows and flexible configuration.

## 8.2 Main differences

| Feature | Hashcat | John the Ripper |
|---|---|---|
| Main purpose | Password recovery and auditing | Password auditing and recovery |
| Hardware | CPU and GPU acceleration | CPU, and GPU support in certain builds and configurations |
| Performance | Highly optimized for GPU workloads | Performance varies by format and build |
| Hash support | Hundreds of hash modes | Hundreds of formats, especially with Jumbo |
| Attack strategies | Dedicated attack-mode system | Wordlist, single, incremental, rules and other modes |
| Hash selection | Often uses explicit `-m` mode | Can automatically detect many formats |
| Configuration | Command-line options and rules | Highly configurable through options and configuration files |
| Best-known strength | Hardware acceleration and extensive hash-mode selection | Flexible password auditing and format handling |

## 8.3 Hashcat attack strategies vs John modes

John the Ripper has different modes, including:

**Single Crack Mode**

Uses information associated with accounts to generate password candidates.

**Wordlist Mode**

Tests candidates from a wordlist and can apply rules.

**Incremental Mode**

Generates combinations of characters according to a configured character set and length.

**External Mode**

Allows customized candidate-generation logic through external mode definitions.

**Mask Mode**

Allows candidate generation based on a specified character pattern in supported versions and configurations.

### Which one is better?

Neither tool is universally better.

Choose Hashcat when GPU acceleration, its supported hash modes, and its attack-mode system are useful.

Choose John the Ripper when its format handling, automatic hash recognition, and flexible auditing workflows are useful.

Many security professionals learn both because they complement each other.

### My learning progression

I first studied John the Ripper to understand offline password auditing. Hashcat builds on that knowledge by introducing hardware acceleration, more candidate-generation strategies, and a different command-line workflow.

---

# 9. Important Hashcat Commands and Options

Hashcat commands follow a general structure:

```bash
hashcat [options] <hash-file> [wordlist or candidate source]
```

### Important options

| Option | Purpose |
|---|---|
| `-m` | Selects the hash mode |
| `-a` | Selects the attack mode |
| `-r` | Applies a rule file |
| `-o` | Saves output to a file |
| `--show` | Displays recovered passwords |
| `--left` | Displays hashes that remain unrecovered |
| `--status` | Displays status information |
| `--restore` | Restores a previous session |
| `--session` | Assigns a session name |
| `-I` | Displays available compute devices |
| `-b` | Runs a benchmark |
| `--example-hashes` | Displays example hash formats |

### Basic examples for an authorized lab

**Check available devices**

```bash
hashcat -I
```

**Run a benchmark**

```bash
hashcat -b
```

**List supported hash modes**

```bash
hashcat -hh
```

**Display recovered results from a test hash file**

```bash
hashcat --show -m 0 test-hashes.txt
```

**Display hashes that remain unrecovered**

```bash
hashcat --left -m 0 test-hashes.txt
```

**Save recovered results to a file during an authorized test**

```bash
hashcat -m 0 -a 0 test-hashes.txt test-wordlist.txt -o recovered.txt
```

Here:

- `-m 0` selects MD5.
- `-a 0` selects straight/wordlist attack.
- `test-hashes.txt` contains authorized test hashes.
- `test-wordlist.txt` contains candidate passwords.
- `-o recovered.txt` saves recovered results.

These examples are for controlled lab data, not accounts or systems without permission.

---

# 10. Advantages and Key Features

Hashcat provides several features that make it a powerful password-auditing tool.

### 1. High Performance

Uses CPU and GPU hardware to accelerate candidate testing.

### 2. Open Source

Hashcat is freely available under the MIT license, allowing users to study and use the software under its license terms.

### 3. Multiple Attack Modes

Supports different candidate-generation approaches, including dictionary, combination, brute-force, PCFG, table, hybrid, generic, and association attacks.

### 4. Wide Hash Support

Supports hundreds of hash modes, including common cryptographic hashes and application-specific password formats.

### 5. Customizable Rules

Rules can modify wordlist entries to create additional password candidates.

### 6. Cross-Platform Support

Supports major operating systems and multiple hardware compute backends.

### 7. Multiple Device Support

Can use multiple compatible devices, including combinations of different hardware types.

### 8. Session Management

Supports pausing, restoring, and continuing interrupted sessions.

### 9. Benchmarking

Provides benchmarking capabilities to evaluate hardware performance for supported hash formats.

### 10. Large-Scale Auditing

Supports multiple hashes and distributed password recovery workflows.

### In short

Hashcat combines hardware acceleration, flexible candidate generation, broad hash support, and configurable workflows in one password-auditing platform.

---

![Benefits of Hashcat](4.jfif)

# 11. Limitations of Hashcat

Although Hashcat is powerful, it has limitations.

### 1. Time-Consuming

Complex passwords may take an extremely long time to identify.

Some passwords may not be recovered within any practical testing period.

### 2. Hardware Dependent

Performance depends on available hardware.

A system with a powerful GPU may perform very differently from a basic computer.

### 3. No Guaranteed Recovery

Hashcat cannot guarantee that a password will be found.

If the candidate-generation strategy never produces the correct password, no match will be found.

### 4. Memory and Resource Requirements

Some hash formats require substantial memory or computational resources.

### 5. Difficult Hash Identification

Choosing the wrong hash mode or providing incorrectly formatted hash data can prevent successful testing.

### 6. Modern Password Hashing Is Expensive

Algorithms such as Argon2id, bcrypt, and scrypt deliberately increase the cost of password guessing.

Their parameters can make large-scale candidate testing significantly harder.

### 7. Learning Curve

Understanding hash modes, attack modes, rules, masks, hardware configuration, and hash formats requires practice.

### 8. Ethical and Legal Risks

Testing password hashes without authorization can violate privacy, organizational policies, or applicable laws.

Always use Hashcat responsibly and within an authorized environment.

![Limitations of Hashcat](5.jfif)
---

# 12. Real-World Applications of Hashcat

Hashcat has applications in cybersecurity, digital forensics, password auditing, and research.

### 1. Penetration Testing

Security professionals can evaluate the strength of password hashes during authorized penetration tests.

### 2. Digital Forensics

Investigators may use password recovery techniques to examine authorized evidence and investigate access to protected data.

### 3. Security Audits

Organizations can assess whether employees are using weak, common, or predictable passwords.

### 4. Incident Response

After a password database exposure, security teams can assess the potential impact and prioritize password resets and other protective measures.

### 5. Cybersecurity Research

Researchers can compare password-hashing algorithms, test computational costs, and investigate password security.

### 6. Personal Password Recovery

Users may attempt to recover their own lost passwords from compatible, legally obtained hash data or other supported recovery material.

### Important reminder

Hashcat does not directly log in to websites, bypass authentication systems, or recover a password from a website simply because its URL is known.

It tests available hash data offline.

![Real world use cases of Hashcat](6.jfif)
---

# 13. Hashcat Learning and Practice Roadmap

This is the learning sequence I recommend for someone who has already studied basic hashing, salting, peppering, and John the Ripper.

## Phase 1 — Understand the Fundamentals

- Understand what Hashcat is.
- Understand its purpose.
- Learn the difference between hashes and passwords.
- Understand offline password auditing.
- Learn the difference between hash modes and attack modes.

## Phase 2 — Learn Hash Formats

- MD5.
- SHA-1.
- SHA-2.
- SHA-3 and Keccak.
- NTLM.
- bcrypt.
- scrypt.
- PBKDF2.
- Application-specific password formats.

## Phase 3 — Understand Attack Modes

Study all ten documented modes:

- Straight.
- Combination.
- Brute-force.
- PCFG.
- Table.
- Hybrid Wordlist + Mask.
- Hybrid Mask + Wordlist.
- Generic.
- Association.
- Hybrid with word-position markers.

Understand when each strategy is useful and what its limitations are.

## Phase 4 — Learn Candidate Generation

- Understand wordlists.
- Understand character sets.
- Learn masks.
- Study rule-based transformations.
- Understand candidate combinations.
- Learn how password patterns affect candidate generation.

## Phase 5 — Understand Hardware Acceleration

- Learn CPU fundamentals.
- Learn GPU fundamentals.
- Understand parallel processing.
- Study OpenCL, CUDA, HIP, and Metal.
- Learn device detection and benchmarking.
- Understand memory and temperature limitations.

## Phase 6 — Compare Hashcat and John the Ripper

- Compare their purposes.
- Understand their attack strategies.
- Compare hash-format support.
- Study CPU and GPU workflows.
- Understand when each tool is appropriate.

## Phase 7 — Practical Lab Work

Use only hashes you create yourself or obtain from authorized training environments.

- Install Hashcat.
- Check hardware compatibility.
- Identify a known test hash format.
- Understand command syntax.
- Practice candidate-generation concepts.
- Review successful and unsuccessful results.
- Learn session management and output handling.

## Phase 8 — Document and Share

- Create simple study notes.
- Prepare visual explanations.
- Document practical exercises.
- Explain what each command does.
- Share the learning journey on GitHub and LinkedIn.

### My learning approach

**Learn → Understand → Practice → Analyze → Document → Share**

---

# 14. Important Security Defenses

Understanding Hashcat also helps us understand how systems can protect passwords.

### 1. Use Strong Passwords

Encourage long, unique passwords rather than common words or predictable combinations.

### 2. Use Modern Password-Hashing Algorithms

Prefer suitable password-storage methods such as Argon2id, bcrypt, scrypt, or PBKDF2 with appropriately configured parameters.

### 3. Use Unique Salts

A unique random salt for every password helps prevent identical passwords from producing identical stored hash records.

### 4. Protect Pepper Values

If peppering is used, keep the pepper separate from the password database and protect it appropriately.

### 5. Enable Multi-Factor Authentication

MFA adds another authentication factor, reducing the risk of password-only compromise.

### 6. Monitor Credential Exposure

Organizations should monitor for compromised credentials and respond quickly to password database exposure.

### 7. Apply Secure Password Policies

Encourage unique passwords, block known-compromised passwords, and support password managers.

### Important point

Strong password storage does not make password guessing mathematically impossible. It makes each guess more expensive and helps reduce the risk of successful recovery.

---

# 15. Final Key Takeaways

Hashcat is a powerful password recovery and auditing tool that helps security professionals evaluate password strength.

### What I learned

- Hashcat is primarily an offline password-auditing tool.
- It tests password candidates against stored hashes.
- It does not decrypt hashes.
- It supports multiple hash algorithms and formats.
- It provides ten documented attack modes.
- It uses `-m` to select a hash format.
- It uses `-a` to select an attack strategy.
- It can use CPU and GPU hardware.
- GPU acceleration can improve performance for suitable workloads.
- Memory-hard algorithms make password guessing more expensive.
- Hashcat and John the Ripper are complementary tools.
- Strong password storage, unique salts, and MFA improve security.

### Final Message

**A powerful password-auditing tool helps us understand weak passwords. Strong security practices help us prevent them from becoming a vulnerability.**

Learn responsibly. Practice ethically. Build securely.

---

# 16. Official Learning Resources

- Hashcat Official Website: https://hashcat.net/hashcat/
- Hashcat GitHub Repository: https://github.com/hashcat/hashcat
- Hashcat Documentation: https://hashcat.net/wiki/
- Hashcat Command Reference: https://hashcat.net/wiki/doku.php?id=hashcat
- John the Ripper Official Documentation: https://www.openwall.com/john/doc/
- OWASP Password Storage Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html

---

## ⚖️ Ethical Use Disclaimer

This repository is created for educational purposes, password security research, and authorized cybersecurity assessments.

Always obtain permission before testing password hashes or conducting security assessments.

Never use recovered credentials to access accounts or systems without authorization.

**Understand the tool. Respect the law. Strengthen security.**
