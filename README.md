# 📧 SMTP Email Traffic Forensics

> **SBT-DF203 - Basic Networking Skills for Digital Forensics**
> **Lab 4: SMTP Email Traffic Forensics**
> **Focus:** Network traffic analysis, SMTP protocol investigation, authentication analysis, email reconstruction, metadata extraction, and encryption assessment.

---

## 🔎 About This Lab

This repository documents a practical forensic examination of captured **SMTP email traffic** using a PCAP network capture.

The exercise focuses on identifying and reconstructing an SMTP email session from network traffic and extracting useful forensic information such as:

* SMTP client and server information
* SMTP commands and server responses
* Authentication activity
* Base64-encoded authentication values
* Sender and recipient information
* Email headers
* Message content and MIME structure
* Attachment information
* Network-layer metadata
* TCP conversation details
* SMTP encryption capabilities
* Evidence of `STARTTLS` usage or non-usage

The analysis was performed in an authorized laboratory environment using a working copy of the captured evidence.

---

## 🎯 Lab Objectives

The main objectives of the exercise were to:

* 🔍 Identify SMTP traffic within a network capture.
* 🌐 Identify the SMTP client and server.
* 🔢 Determine the TCP ports used by the SMTP session.
* 🧩 Reconstruct the SMTP command and response sequence.
* 🔐 Examine the authentication mechanism used by the client.
* 🔤 Identify Base64-encoded authentication values.
* ✉️ Reconstruct the captured email message.
* 📝 Extract relevant email headers and client information.
* 📦 Identify MIME content and attachments.
* 🖥️ Record IP addresses, TCP ports, Ethernet/MAC information and related metadata.
* 🛡️ Determine whether TLS was actually negotiated during the observed session.
* 📋 Preserve evidence integrity and document the analysis process.

---

## 🧪 Laboratory Environment

The analysis was performed using **Kali Linux**.

### 🛠️ Tools Used

| Tool            | Purpose                                              |
| --------------- | ---------------------------------------------------- |
| 🦈 Wireshark    | Graphical packet inspection and TCP stream analysis  |
| 🔬 TShark       | Command-line packet extraction and protocol analysis |
| 🐍 Python 3     | Offline Base64 decoding and supporting analysis      |
| 🔑 SHA-256      | Evidence integrity verification                      |
| 📊 Capinfos     | PCAP file and capture metadata inspection            |
| 🖥️ Linux shell | Evidence management and analysis workflow            |
| 📦 Git/GitHub   | Version control and laboratory documentation         |

---

## 📂 Repository Structure

```text
SMTP-Email-Traffic-Forensics/
│
├── 📁 evidence/
│   └── smtp.pcap
│
├── 📁 working/
│   └── smtp_working.pcap
│
├── 📁 exported/
│   └── Extracted or reconstructed forensic artefacts
│
├── 📁 reports/
│   ├── smtp_capture_hashes.txt
│   ├── smtp_capinfos.txt
│   ├── smtp_authentication.txt
│   └── Additional analysis outputs
│
├── 📁 screenshots/
│   ├── 01_lab_preparation_and_verification.png
│   ├── 02_tool_verification.png
│   ├── 03_packet_capture_download.png
│   ├── 04_creation_of_working_copy.png
│   ├── 05_sha256_integrity_verification.png
│   ├── 06_capture_file_characteristics.png
│   ├── 07_capture_metadata_recorded.png
│   ├── 08_working_pcap_in_wireshark.png
│   ├── 09_smtp_traffic_filter.png
│   ├── 10_tcp_conversation_inventory.png
│   ├── 11_smtp_command_and_response_inventory.png
│   ├── 12_smtp_authentication_base64_credentials.png
│   ├── 13_offline_base64_decoding.png
│   ├── 14_python_verification.png
│   ├── 15_identification_of_tcp_stream_0.png
│   ├── 16_tshark_stream_packet_extraction.png
│   ├── 17_smtp_message_data_reassembly_fields.png
│   ├── 18_tcp_payload_segmentation.png
│   ├── 19_reconstructed_smtp_conversation_email_content.png
│   ├── 20_smtp_client_server_network_metadata.png
│   ├── 21_tls_packet_verification.png
│   ├── 22_starttls_authentication_verification.png
│   ├── 23_email_content_extracted.png
│   ├── 24_multipart_evidence.png
│   ├── 25_smtp_advertisement.png
│   ├── 26_tshark_verification.png
│   ├── 27_wireshark_verification.png
│   └── 28_capture_hash_capinfos.png
│
├── 📁 scripts/
│   └── Supporting analysis scripts
│
└── 📄 README.md
```

The directory structure separates the original evidence, working copy, generated artefacts, reports, screenshots and supporting scripts.

---

## 🔐 Evidence Handling and Integrity

Evidence handling was performed before detailed analysis.

The original capture was preserved under:

```text
evidence/smtp.pcap
```

A separate working copy was created under:

```text
working/smtp_working.pcap
```

The working copy was used for analysis while the original capture was retained as the source evidence.

### 🔑 SHA-256 Verification

The SHA-256 hash of both files was calculated and compared.

```text
Original evidence:
17ad230db1b6fd5dd18eb311092df1cf6eb162054bdb47697b89bef5a86a47ab

Working copy:
17ad230db1b6fd5dd18eb311092df1cf6eb162054bdb47697b89bef5a86a47ab
```

The matching hashes demonstrate that the working copy was identical to the acquired evidence at the point of verification.

The hash output is preserved in:

```text
reports/smtp_capture_hashes.txt
```

---

## 📊 Capture Overview

The capture was examined using `capinfos`.

### Capture characteristics

| Property               | Observation                    |
| ---------------------- | ------------------------------ |
| 📦 File format         | PCAP                           |
| 🌐 Encapsulation       | Ethernet                       |
| 📦 Number of packets   | 60                             |
| 💾 File size           | Approximately 27 kB            |
| ⏱️ Capture duration    | 9.198384 seconds               |
| 🕐 Earliest packet     | 2009-10-05 02:06:07.492060     |
| 🕐 Latest packet       | 2009-10-05 02:06:16.690444     |
| 📡 Interfaces          | 1                              |
| 📈 Average packet size | 447.77 bytes                   |
| 📊 Average packet rate | Approximately 6 packets/second |
| ✅ Strict time order    | True                           |

The complete `capinfos` output is retained in:

```text
reports/smtp_capinfos.txt
```

---

## 🌐 SMTP Session Identified

The capture contains a TCP conversation between:

```text
Client: 10.10.1.4:1470
Server: 74.53.140.153:25
```

The SMTP server uses **TCP port 25**.

The identified TCP conversation contained:

```text
53 TCP frames
```

with approximately:

```text
24 kB of traffic
```

and a duration of approximately:

```text
7.58 seconds
```

The SMTP server identified itself as:

```text
Exim 4.69
```

---

## ✉️ SMTP Conversation

The captured session follows the expected SMTP workflow.

### 🔹 Server Greeting

The SMTP server begins the session with a `220` response.

```text
220-xc90.websitewelcome.com ESMTP Exim 4.69
```

The greeting establishes the SMTP service and identifies the server software.

---

### 🔹 EHLO

The client sends:

```text
EHLO GP
```

The server responds with capabilities including:

```text
SIZE 52428800
PIPELINING
AUTH PLAIN LOGIN
STARTTLS
HELP
```

This is significant because the server advertises both authentication mechanisms and TLS capability.

---

### 🔐 Authentication

The client requests:

```text
AUTH LOGIN
```

The server responds with Base64-encoded prompts for the username and password.

The client then sends two Base64-encoded values.

The authentication exchange ends with:

```text
235 Authentication succeeded
```

This confirms that the authentication attempt was successful.

The authentication values themselves are retained in the forensic evidence but should be treated as sensitive information.

---

## 🔤 Base64 Authentication

The captured authentication exchange demonstrates the use of Base64 encoding.

The server sends:

```text
334 VXNlcm5hbWU6
```

and:

```text
334 UGFzc3dvcmQ6
```

These represent the prompts:

```text
Username:
Password:
```

The client subsequently sends Base64-encoded authentication values.

### ⚠️ Security Observation

Base64 is **encoding, not encryption**.

Therefore, Base64-encoded authentication material can be reversibly decoded when captured from an unencrypted session.

For this reason, authentication values should not be unnecessarily exposed in public documentation or screenshots.

---

## 📨 Email Transaction

After successful authentication, the client submits the email.

The SMTP transaction follows:

```text
MAIL FROM
    ↓
250 OK
    ↓
RCPT TO
    ↓
250 Accepted
    ↓
DATA
    ↓
354 Enter message
    ↓
Email content
    ↓
250 OK
```

The captured sender and recipient were identified from the SMTP envelope.

### 📤 Sender

```text
gurpartap@patriots.in
```

### 📥 Recipient

```text
raj_deol2002in@yahoo.co.in
```

These values are included here as part of the forensic evidence documented during the laboratory analysis.

---

## 📝 Email Headers

The reconstructed message contains standard email headers including:

```text
From
To
Subject
Date
Message-ID
MIME-Version
Content-Type
```

The message subject was:

```text
SMTP
```

The captured client information identifies:

```text
X-Mailer: Microsoft Office Outlook 12.0
```

This provides useful evidence about the software used to generate the email.

---

## 🖥️ Email Client Identification

The email contains the following client indicator:

```text
Microsoft Office Outlook 12.0
```

The `X-Mailer` header provides an application-level indicator that can assist with identifying the originating mail client.

Such metadata can be useful when correlating network evidence with endpoint or user activity.

---

## 📦 MIME Structure

The message uses:

```text
Content-Type: multipart/mixed
```

The reconstructed email contains both:

* 📝 Plain-text content
* 🌐 HTML content
* 📎 An attachment

The attachment identified in the message is:

```text
NEWS.txt
```

The attachment content consists of a **Dev-C++ version/change log**.

The presence of MIME boundaries and content-transfer encoding information allows the message structure and attachment to be reconstructed from the SMTP stream.

---

## 🧩 Email Reconstruction

The SMTP `DATA` section contains the actual email message.

The reconstructed message includes:

```text
Email headers
       ↓
MIME structure
       ↓
Plain-text body
       ↓
HTML body
       ↓
Attachment
```

This demonstrates that SMTP traffic analysis can expose not only the SMTP envelope information but also the contents and structure of an email when the session is not protected by encryption.

---

## 🌐 Network Metadata

The principal network endpoints identified during the SMTP session were:

```text
Client IP:        10.10.1.4
Client TCP port: 1470

Server IP:        74.53.140.153
Server TCP port:  25
```

The TCP conversation was identified as the primary SMTP communication channel in the capture.

Ethernet source and destination information was also examined in Wireshark as part of the network metadata analysis.

---

## 🔒 SMTP Encryption Analysis

The SMTP server advertised:

```text
STARTTLS
```

as one of its capabilities.

However, **advertising STARTTLS does not by itself prove that TLS was used**.

For TLS to be established during the observed session, the SMTP conversation would need to contain a `STARTTLS` command followed by the corresponding TLS negotiation.

The captured SMTP conversation instead proceeds through:

```text
EHLO
AUTH LOGIN
MAIL FROM
RCPT TO
DATA
```

without an observed `STARTTLS` command before the authentication and message exchange.

This is an important distinction in forensic analysis:

> The availability of an encryption capability is not evidence that encryption was actually negotiated.

---

## ⚠️ Security and Forensic Observations

Several security-relevant observations were identified from the capture.

### 🔴 Authentication Exposure

The session uses:

```text
AUTH LOGIN
```

with Base64-encoded authentication values.

Base64 does not provide confidentiality.

### 🔴 Email Content Exposure

The SMTP message, including headers, body and attachment information, can be reconstructed from the captured traffic.

### 🟠 STARTTLS Advertisement

The server advertises:

```text
STARTTLS
```

but the observed session does not show a corresponding `STARTTLS` command before authentication and message transmission.

### 🟢 Successful Message Delivery

The server acknowledges the message with:

```text
250 OK
```

followed by a message identifier.

### 🟢 Clean Session Termination

The client sends:

```text
QUIT
```

and the server responds:

```text
221 xc90.websitewelcome.com closing connection
```

---

## 🔬 Forensic Methodology

The investigation followed a controlled workflow:

```text
📥 Evidence Acquisition
        ↓
🔑 SHA-256 Integrity Verification
        ↓
📊 PCAP Metadata Examination
        ↓
🦈 Wireshark Traffic Review
        ↓
🌐 TCP Conversation Identification
        ↓
📧 SMTP Traffic Filtering
        ↓
🔍 Command/Response Extraction
        ↓
🔐 Authentication Analysis
        ↓
📝 Email Reconstruction
        ↓
📦 MIME and Attachment Analysis
        ↓
🌐 Network Metadata Examination
        ↓
🔒 Encryption Assessment
        ↓
📋 Findings and Documentation
```

The analysis was performed against the working copy while preserving the original evidence.

---

## 📸 Evidence Screenshots

Screenshots are maintained under:

```text
screenshots/
```

The evidence set covers the major stages of the investigation, including:

* 🔑 Evidence hashes and capture metadata
* 📧 SMTP conversation identification
* 📨 SMTP `220` server greeting
* 🔄 EHLO and SMTP capabilities
* 🔐 AUTH LOGIN exchange
* 🔤 Base64 authentication values
* ✉️ MAIL FROM / RCPT TO / DATA sequence
* 📝 Follow TCP Stream reconstruction
* 🖥️ Email headers and client identification
* 🌐 IP addresses, ports and MAC information
* 🔒 STARTTLS/TLS assessment

Sensitive authentication information should be masked in screenshots intended for external publication or submission where required.

---

## 📁 Analysis Outputs

Generated analysis outputs are maintained under:

```text
reports/
```

Examples include:

```text
smtp_capture_hashes.txt
smtp_capinfos.txt
smtp_authentication.txt
```

These files preserve command-line outputs used to support the findings documented in the report.

---

## 🧠 Key Findings

The investigation established that:

* 📧 The captured traffic contains an SMTP email session.
* 🌐 The SMTP server is `74.53.140.153` and listens on TCP port `25`.
* 🖥️ The client is `10.10.1.4` using ephemeral TCP port `1470`.
* 📨 The SMTP server identifies itself as **Exim 4.69**.
* 🔐 The session uses `AUTH LOGIN`.
* 🔤 Authentication values are transmitted using Base64 encoding.
* ✅ Authentication succeeds with SMTP response `235`.
* ✉️ The message contains identifiable sender and recipient envelope information.
* 📝 The subject is `SMTP`.
* 🖥️ The email identifies **Microsoft Office Outlook 12.0** as the mail client.
* 📦 The message uses `multipart/mixed` MIME formatting.
* 📝 Both plain-text and HTML message content are present.
* 📎 The message contains a `NEWS.txt` attachment.
* 💻 The attachment contains Dev-C++ version/change-log information.
* ✅ The SMTP server accepts the message with `250 OK`.
* 🔒 `STARTTLS` is advertised by the server.
* ⚠️ The observed conversation does not show `STARTTLS` being negotiated before authentication and message transmission.
* 🚪 The session terminates through `QUIT` followed by the server's `221` response.

---

## 🛡️ Limitations

The analysis is limited to information observable within the supplied PCAP.

The capture does not by itself establish:

* 👤 The real-world identity of the person operating the client.
* 🏢 The physical location of the user.
* 💻 The complete configuration or security posture of the endpoint.
* 🔑 The validity or continued use of the captured credentials outside the laboratory context.
* 🌐 Events occurring outside the captured time window.
* 🔒 Encryption status of traffic that is not present in the supplied capture.

Conclusions are therefore limited to the network evidence available in the PCAP.

---

## 📚 Evidence Preservation

The original capture is retained separately from the working copy:

```text
evidence/smtp.pcap
working/smtp_working.pcap
```

The SHA-256 values of both files were verified as identical:

```text
17ad230db1b6fd5dd18eb311092df1cf6eb162054bdb47697b89bef5a86a47ab
```

This supports the integrity of the working copy used during the investigation.

---

## 📝 Report

The detailed laboratory report contains the full forensic workflow, screenshots, observations, analysis and conclusions.

The final report is structured around:

```text
📌 Executive Summary
🔐 Evidence Integrity and Scope
📧 SMTP Timeline
🔄 SMTP Command and Response Analysis
🔑 Authentication and Redaction
📝 Email Reconstruction
🌐 Client, Server and Network Metadata
🔒 Encryption and Limitations
📋 Conclusions
📎 Appendices and Evidence
```

---

## ⚖️ Ethical and Legal Considerations

This analysis was conducted for authorized digital-forensics laboratory training.

Captured network traffic may contain authentication material, email addresses, message content and other potentially sensitive information.

Accordingly:

* 🔒 Evidence should be handled only within an authorized environment.
* 🧾 Original evidence should be preserved.
* 🧪 Analysis should be performed on working copies where practical.
* 🔑 Hashes should be used to support evidence integrity.
* 🚫 Credentials should not be reused or exposed unnecessarily.
* 🕵️ Sensitive personal information should be masked in public-facing documentation where appropriate.
* 📤 Evidence should not be redistributed outside the authorized purpose of the exercise.

---

## 🎓 Academic Context

**Course:** SBT-DF203 - Basic Networking Skills for Digital Forensics

**Laboratory:** Lab 4 - SMTP Email Traffic Forensics

**Primary forensic focus:**

```text
Network Forensics
        +
SMTP Protocol Analysis
        +
Email Traffic Reconstruction
        +
Authentication Analysis
        +
Digital Evidence Handling
```

---

## 👩🏽‍💻 Author

**Kafayat Omolara Animashawun**

Cybersecurity Professional
CISSP

This repository represents practical laboratory work in network traffic analysis and digital forensics.

---

## ⭐ Repository Purpose

This repository serves as a structured record of the SMTP Email Traffic Forensics laboratory exercise, including:

```text
📦 Evidence
🔬 Analysis
📊 Extracted Results
📸 Screenshots
📝 Reports
🐍 Supporting Scripts
📚 Documentation
```

The objective is to demonstrate a repeatable and evidence-focused approach to investigating SMTP traffic captured from a network environment.
