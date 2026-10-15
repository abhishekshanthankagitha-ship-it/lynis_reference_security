# Lynis Security Enhancements

### Custom Security Auditing for Login Banners and NTP Configuration
A cybersecurity project focused on studying existing security checks in [Lynis](https://github.com/CISOfy/lynis) and developing custom Bash scripts to enhance login banner validation and Network Time Protocol (NTP) security auditing on Linux systems.

---

## 📌 Project Overview

[Lynis](https://github.com/CISOfy/lynis) is an open-source security auditing tool for Linux, macOS, and Unix-based systems. It performs security checks, identifies potential weaknesses, and provides recommendations to improve system security.

This project examines selected Lynis tests and develops standalone Bash scripts that perform additional security checks in two areas:

- **Login Banner Security:** Examines login banner files for appropriate warning messages and potential security concerns.
- **NTP Security Auditing:** Examines time synchronization configuration and runtime status to identify potential configuration weaknesses.

The project documents the original Lynis reference tests, custom enhancements, and corresponding execution results.

> **Implementation note:** The custom scripts are standalone Bash utilities inspired by Lynis security checks. They are not integrated into the Lynis test execution framework.

---

## 🎯 Project Objectives

- Study existing Lynis security tests and their implementation.
- Understand how Linux security auditing checks are performed.
- Identify opportunities to extend existing security validation.
- Develop custom Bash scripts for login banner and NTP security auditing.
- Compare the original reference checks with the custom implementations.
- Record execution results for documentation and evaluation.

---

## 🛡️ Security Areas

### 1. Login Banner Security

**Original Lynis reference test:** `BANN-7126`

The original test checks `/etc/issue` for legal warning keywords intended to discourage unauthorized system access.

The custom login banner script extends the basic validation with additional checks, including:

- Checking whether `/etc/issue` and `/etc/issue.net` exist.
- Identifying empty or missing banner files.
- Checking for required warning-related keywords.
- Detecting potential credential-like assignments using heuristic patterns.
- Reporting findings to help administrators review banner configuration.

**Security objective:** Improve the visibility of login banner weaknesses and encourage the use of appropriate warning messages.

### 2. NTP Security Auditing

**Original Lynis reference tests:** `TIME-3104`, `TIME-3106`, `TIME-3170`, and `TIME-3185`

These tests cover areas such as time synchronization services, configuration-file security, and systemd-timesyncd synchronization.

The custom NTP script performs additional checks, including:

- Inspecting relevant time synchronization configuration files and drop-in directories.
- Checking for explicit NTP server and fallback configuration entries.
- Examining runtime time synchronization information.
- Comparing detected server hostnames against a configured hostname allowlist.
- Checking whether `systemd-timesyncd` is active.
- Checking the system's NTP synchronization status.

**Security objective:** Help administrators review time synchronization configuration and identify potential weaknesses or unexpected settings.

---

## 📂 Repository Structure

```text
lynis_reference_security/
│
├── README.md
│
├── reference_scripts/
│   ├── login_banner_reference.txt
│   └── ntp_reference.txt
│
├── enhanced_scripts/
│   ├── custom_login_banner.sh
│   └── custom_ntp_security.sh
│
└── results/
    ├── reference_login_banner_output.txt
    └── reference_ntp_output.txt
```

The reference files document the relevant original Lynis test code. The results files contain selected log entries from the original Lynis audit.

Custom script execution results and screenshots can be added to the `results/` directory as testing progresses.

---

## ⚙️ Technologies Used

- **Operating System:** Kali Linux
- **Scripting Language:** Bash
- **Security Auditing Tool:** Lynis
- **System Utilities:** `grep`, `sed`, `timedatectl`
- **Version Control:** Git and GitHub

---

## 🚀 Execution

### Prerequisites

- A Linux environment, such as Kali Linux.
- Bash shell.
- Lynis installed for running the original reference audit.
- Appropriate permissions to inspect system configuration and time synchronization status.

### 1. Run the Original Lynis Audit

```bash
sudo lynis audit system
```

Search the Lynis log for the login banner reference test:

```bash
sudo grep -i -E 'BANN-7126|issue banner|legal key words' /var/log/lynis.log
```

Search for the relevant time synchronization tests:

```bash
sudo grep -i -E 'TIME-3104|TIME-3106|TIME-3170|TIME-3185' /var/log/lynis.log
```

### 2. Run the Custom Login Banner Script

After placing the script in `enhanced_scripts/`, run:

```bash
bash enhanced_scripts/custom_login_banner.sh
```

### 3. Run the Custom NTP Security Script

```bash
bash enhanced_scripts/custom_ntp_security.sh
```

**Note:** The custom script paths above correspond to the intended repository structure. Ensure the files have been uploaded to those locations before running the commands. Review the scripts before execution and use appropriate permissions when necessary.

---

## 📊 Testing and Results

The project evaluates the original Lynis tests and the custom scripts separately.

| Test Area | Original Reference | Custom Implementation |
|---|---|---|
| Login banner | `BANN-7126` | Additional banner validation |
| NTP service detection | `TIME-3104` | Runtime service checks |
| NTP synchronization | `TIME-3106`, `TIME-3185` | Synchronization status checks |
| Configuration-file security | `TIME-3170` | Additional configuration inspection |

Original Lynis audit output is documented separately from custom script output to distinguish the baseline checks from the additional validation.

Results depend on the system configuration, installed services, and available runtime information.

---

## 🔐 Security Considerations

- Run security audits only on systems you own or are authorized to assess.
- Review scripts before executing them with elevated privileges.
- Treat credential-like pattern matches as potential indicators requiring manual verification, not proof of exposed credentials.
- A hostname allowlist is only as reliable as its configuration and validation.
- Missing explicit NTP configuration does not necessarily indicate a vulnerability because a system may obtain defaults through another configuration mechanism.
- A synchronized system clock does not, by itself, prove that the configured time source is trustworthy.

---

## 🔮 Future Enhancements

- Integrate the custom checks into the Lynis test framework.
- Add more detailed reporting and severity classification.
- Improve validation of NTP server configuration and runtime behavior.
- Add automated test cases for missing, empty, and misconfigured banner files.
- Compare original and enhanced test results in a consistent report format.
- Add repeatable test scenarios and documented expected outcomes.

---

## 📚 References

- [Lynis Official Repository](https://github.com/CISOfy/lynis)
- [Lynis Banner Tests](https://github.com/CISOfy/lynis/blob/master/include/tests_banners)
- [Lynis Time-Related Tests](https://github.com/CISOfy/lynis/blob/master/include/tests_time)
- [Lynis Test Database](https://github.com/CISOfy/lynis/blob/master/db/tests.db)

The source code available in the installed Lynis version may differ from the current upstream repository. The reference files in this project should correspond to the version used during development.

---

## 👨‍💻 Project Information

**Project:** Lynis Security Enhancements  
**Focus:** Linux Security Auditing and Bash Scripting  
**Areas:** Login Banner Security and NTP Configuration Auditing

---

*This project is intended for educational purposes and authorized security auditing. The custom scripts provide additional checks and should not be considered a replacement for a complete security audit.*
