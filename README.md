# Lynis Reference Security Tests

### Login Banner Security and NTP Configuration Auditing

A cybersecurity project documenting selected reference tests from **Lynis**, an open-source security auditing tool for Linux and Unix-based systems. This repository focuses on login banner security and Network Time Protocol (NTP) configuration auditing.

The repository contains the original Lynis reference code and separate output files documenting the corresponding audit results.

---

## 1. Project Overview

[Lynis](https://github.com/CISOfy/lynis) is a security auditing tool that examines system configurations, identifies potential security weaknesses, and provides recommendations for improving system security.

This project studies selected Lynis tests related to:

- **Login Banner Security:** Examining legal warning messages displayed to users before login.
- **NTP Security:** Examining time synchronization services, configuration-file security, and system clock synchronization.

The original test implementations are documented alongside their audit outputs to provide a clear reference for understanding how Lynis performs these security checks.

---

## 2. Project Objectives

- Study the implementation of selected Lynis security tests.
- Understand how Linux security auditing checks are performed.
- Examine login banner configuration and warning-message validation.
- Understand NTP service detection and time synchronization checks.
- Document the original reference code separately from its execution output.
- Establish a baseline for studying additional security validation techniques.

---

## 3. Reference Test 1: Login Banner Security

### Test ID: BANN-7126

**Test description:** Check the contents of `/etc/issue` for legal warning keywords.

### Purpose

The `BANN-7126` test examines the `/etc/issue` file to determine whether it contains warning-related keywords intended to discourage unauthorized users from accessing the system.

A suitable login banner can communicate that a system is restricted to authorized users and that unauthorized access is prohibited.

### How the Test Works

1. Checks whether `/etc/issue` exists.
2. Registers the test with the Lynis auditing framework.
3. Examines the file for configured legal banner keywords.
4. Counts the matching keywords.
5. Displays a result based on the configured keyword threshold.
6. Provides a recommendation when the banner may require improvement.

### Reference Source Code

The original test implementation is available in:

`reference_scripts/login_banner_reference.txt`

### Execution Output

The corresponding Lynis audit log entries are available in:

`results/reference_login_banner_output.txt`

The output includes the test identification, the banner-content check, and any recommendation recorded by Lynis.

---

## 4. Reference Test 2: NTP Security

This section documents selected Lynis tests associated with time synchronization and configuration security.

### TIME-3104: NTP Service Detection

**Purpose:** Checks for a running NTP daemon or client.

This test helps determine whether an NTP-related service or client is running on the system.

### TIME-3106: Systemd NTP Synchronization Status

**Purpose:** Checks systemd NTP time synchronization status.

The test examines synchronization-related information when its prerequisites are met.

### TIME-3170: Configuration-File Security

**Purpose:** Checks permissions and ownership of configuration files.

The test examines relevant time-related configuration files to identify potentially insecure permissions or ownership settings.

### TIME-3185: Systemd-Timesyncd Synchronization

**Purpose:** Checks systemd-timesyncd synchronization status.

This test checks whether the system clock has been synchronized through the systemd-timesyncd service.

### Reference Source Code

The original NTP-related test implementation is documented in:

`reference_scripts/ntp_reference.txt`

### Execution Output

The corresponding Lynis audit log entries are available in:

`results/reference_ntp_output.txt`

The output records which relevant tests were performed or skipped during the audit. A test being performed does not, by itself, establish whether the system passed or failed the security check.

---

## 5. Repository Structure

```text
lynis_reference_security/
│
├── README.md
│
├── reference_scripts/
│   ├── login_banner_reference.txt
│   └── ntp_reference.txt
│
└── results/
    ├── reference_login_banner_output.txt
    └── reference_ntp_output.txt
```

### Directory Description

| File or Directory | Description |
|---|---|
| `README.md` | Project overview, test descriptions, and documentation |
| `reference_scripts/` | Contains the original Lynis reference test code |
| `login_banner_reference.txt` | Reference implementation of test BANN-7126 |
| `ntp_reference.txt` | Reference implementations of selected NTP-related tests |
| `results/` | Contains the corresponding audit output files |
| `reference_login_banner_output.txt` | Login banner reference audit log entries |
| `reference_ntp_output.txt` | NTP reference audit log entries |

---

## 6. Environment and Tools

| Component | Details |
|---|---|
| Operating System | Kali Linux |
| Security Auditing Tool | Lynis |
| Scripting Language | Bash |
| Text Processing Utilities | `grep`, `sed` |
| Time Synchronization Utility | `timedatectl` |
| Version Control | Git and GitHub |

---

## 7. Running the Reference Audit

### Prerequisites

- A Linux system with Lynis installed.
- Appropriate permissions to execute a system security audit.
- Access to the relevant system configuration and log files.

### Step 1: Run the Lynis Audit

```bash
sudo lynis audit system
```

Allow the audit to complete.

### Step 2: Find Login Banner Test Results

```bash
sudo grep -i -E 'BANN-7124|BANN-7126|BANN-7128|BANN-7130|issue banner|legal key words' /var/log/lynis.log
```

This searches the Lynis log for login banner-related test entries.

### Step 3: Find NTP Test Results

```bash
sudo grep -i -E 'TIME-3104|TIME-3106|TIME-3170|TIME-3185' /var/log/lynis.log
```

This searches the log for the selected NTP and time synchronization test entries.

### Step 4: Inspect the Saved Output Files

Display the login banner output:

```bash
cat results/reference_login_banner_output.txt
```

Display the NTP output:

```bash
cat results/reference_ntp_output.txt
```

The saved files contain selected log entries from the audit performed on the development system. To obtain updated results, run the audit again and regenerate the output files.

---

## 8. Understanding the Results

The audit output may contain messages indicating that a test was performed, skipped, or accompanied by a recommendation.

These messages must be interpreted carefully:

- **Test performed:** Lynis executed the corresponding check.
- **Test skipped:** The check was not executed, potentially because its prerequisites were not met.
- **Recommendation recorded:** Lynis identified an issue or an opportunity for improvement that warrants further review.
- **No explicit configuration found:** This does not necessarily indicate a vulnerability; the system may obtain configuration values through another mechanism.

The execution log should be interpreted alongside the corresponding test implementation and, where available, the detailed Lynis report.

---

## 9. Security Significance

### Login Banner Security

Login banners help communicate system access restrictions and discourage unauthorized use. Administrators should ensure that warning messages are appropriate for the environment and comply with organizational policies.

### NTP Security

Reliable and secure time synchronization is important for accurate log timestamps, authentication mechanisms, certificate validation, and incident investigation.

Administrators should review time synchronization configuration, service status, server trust, and system synchronization status when assessing NTP security.

A synchronized clock alone does not prove that the configured time source is trustworthy.

---

## 10. Limitations

- The repository documents selected Lynis tests rather than the complete Lynis auditing framework.
- The saved outputs represent the audit results captured on the development system.
- Test behavior may vary depending on the installed Lynis version, operating system, service configuration, and test prerequisites.
- The presence of a test execution message does not establish that the check passed.
- The repository contains reference code and corresponding outputs; it does not claim that custom enhancements have been integrated into Lynis.

---

## 11. References

- [Official Lynis Repository](https://github.com/CISOfy/lynis)
- [Lynis Banner Tests](https://github.com/CISOfy/lynis/blob/master/include/tests_banners)
- [Lynis Time-Related Tests](https://github.com/CISOfy/lynis/blob/master/include/tests_time)
- [Lynis Test Database](https://github.com/CISOfy/lynis/blob/master/db/tests.db)

**Version note:** The reference source files should correspond to the Lynis version installed during testing. The upstream repository may contain newer or different implementations.

---

## 12. Conclusion

This repository documents selected Lynis security checks related to login banner configuration and NTP security.

By organizing the original reference code and its corresponding audit output into separate files, the project provides a structured resource for studying Linux security auditing, understanding test behavior, and reviewing the results generated by Lynis.

The documented tests provide a foundation for further work on Linux system hardening and security validation.

---

**Project:** Lynis Reference Security Tests  
**Focus Areas:** Login Banner Security and NTP Configuration Auditing  
**Platform:** Kali Linux  
**Purpose:** Cybersecurity learning, reference documentation, and authorized security auditing
