---
title: Rocky Linux 9 STIG
description: 
tags: RockyLinux/Stig RockyLinux9/Stig STIGViewer SCAP/oscap linux/hardening
date: 2024-08-16
time: 10:08
---

# Stig

> [!info]- Abbreviations
> ### **1. STIG - Security Technical Implementation Guide**
>
>- **STIG:** A document provided by DISA (Defense Information Systems Agency) that outlines security configuration requirements for various technologies and platforms to ensure compliance with security policies and standards.
>
>### **2. SRG - Security Requirements Guide**
>
>- **SRG:** A document that provides high-level security requirements for specific types of systems, such as operating systems or network devices. SRGs are used as the foundation for developing STIGs and are often referenced in STIG documents.
>
>### **3. CCI - Control Correlation Identifier**
>
>- **CCI:** A unique identifier used to cross-reference and link security controls across different frameworks and standards. CCIs help in mapping and correlating controls from different sources to ensure comprehensive coverage.
>
>### **4. SCAP - Security Content Automation Protocol**
>
>- **SCAP:** A suite of standards used to automate the assessment of security configurations and compliance. SCAP includes several components like XCCDF, OVAL, and CVE, which are used to standardize and automate security assessments.
>
>### **5. XCCDF - Extensible Configuration Checklist Description Format**
>
>- **XCCDF:** A standard format used for representing security checklists and benchmarks. XCCDF is part of the SCAP suite and is used to describe the rules and tests for compliance checks.
>
>### **6. OVAL - Open Vulnerability and Assessment Language**
>
>- **OVAL:** A standard for encoding and sharing information about computer security vulnerabilities. It provides a language for describing security-related information and is used in conjunction with XCCDF to assess system configurations.
>
>### **7. CVE - Common Vulnerabilities and Exposures**
>
>- **CVE:** A standardized identifier for known security vulnerabilities and exposures. CVE identifiers are used to reference and track specific vulnerabilities across various systems and databases.
>
>### **8. FIPS - Federal Information Processing Standards**
>
>- **FIPS:** A set of standards and guidelines developed by NIST (National Institute of Standards and Technology) for information security and processing. FIPS standards often form the basis for security requirements in government and military environments.
>
>### **9. DISA - Defense Information Systems Agency**
>
>- **DISA:** A United States Department of Defense agency responsible for providing IT and cybersecurity support. DISA publishes STIGs and other security guidelines to enhance the security of IT systems.
>
>### **10. NIST - National Institute of Standards and Technology**
>
>- **NIST:** A U.S. federal agency that develops standards and guidelines for technology, including cybersecurity. NIST’s guidelines and frameworks often underpin security requirements and compliance standards.
>
>### **11. IAVA - Information Assurance Vulnerability Alert**
>
>- **IAVA:** Alerts issued by DISA to inform about vulnerabilities in systems and provide guidance on mitigating these vulnerabilities.
>
>### **12. NSA - National Security Agency**
>
>- **NSA:** A U.S. government agency that, among other roles, provides cybersecurity guidance and standards. NSA’s guidelines may be referenced in conjunction with STIGs and other security frameworks.
>
>### **13. RMF - Risk Management Framework**
>
>- **RMF:** A framework developed by NIST for managing risks associated with information systems. RMF provides a structured approach to risk management and integrates with STIGs for security compliance.
>
>### **14. ATO - Authorization to Operate**
>
>- **ATO:** A formal declaration by an authorizing official that an IT system is approved to operate based on its compliance with security requirements and risk assessments.
>
>### **15. SCAP Security Guide (SSG)**
>- **SSG** provides a comprehensive set of security configuration checklists and benchmarks for various systems and applications. It aims to help organizations comply with security standards and best practices by offering pre-configured security settings and guidelines.

## SSG

**SSG** stands for **SCAP Security Guide**. It is a collection of security configuration benchmarks and guides based on the Security Content Automation Protocol (SCAP). Here’s a detailed look at what SSG is and its components:

#### **Components**

1. **Security Content**:
    
    - **SSG** contains predefined security profiles for different operating systems, applications, and devices. These profiles are based on various security standards and guidelines, such as DISA STIGs and NIST guidelines.
2. **Configuration Guides**:
    
    - It includes configuration guides that outline recommended settings and controls for achieving compliance with security requirements. These guides are often tailored to specific technologies, such as Linux distributions, Windows, or network devices.
3. **Benchmark Files**:
    
    - **SSG** provides benchmark files in formats such as XCCDF (Extensible Configuration Checklist Description Format) and OVAL (Open Vulnerability and Assessment Language). These benchmarks describe the security requirements and checks that should be applied to systems.
4. **Remediation Scripts**:
    
    - It includes scripts and instructions for automating the application of security settings and remediation of vulnerabilities. These scripts help in applying the recommended configurations to systems and validating their compliance.

## CATEGORIES

### **CAT 1 - Critical Controls**

**CAT 1** controls are considered **Critical**. These controls address vulnerabilities or issues that have a severe impact on the security of the system and are critical for maintaining the security posture of the system. If these controls are not properly implemented or are non-compliant, it could lead to significant security risks or breaches.

### **CAT 2 - Important Controls**

**CAT 2** controls are considered **Important**. These controls address significant security issues but are not as critical as CAT 1 controls. While not as immediately impactful as CAT 1, failure to address CAT 2 controls can still lead to security vulnerabilities and may affect the overall security posture of the system.

### **CAT 3 - General Controls**

**CAT 3** controls are considered **General**. These controls address issues that are less critical compared to CAT 1 and CAT 2 but still contribute to overall security. They are often best practices or general recommendations for improving security posture.


# STIG Viewer

# SCAP

## oscap

*  Information 

```shell
oscap info /usr/share/xml/scap/ssg/content/ssg-rhel7-ds.xml
```


* Evaluate and create reports and results

`--profile` `found with oscap info`

```shell
sudo oscap xccdf eval \
--fetch-remote-resources \
--profile xccdf_org.ssgproject.content_profile_stig \
--results stig-results.xml \
--report stig-report.html \
/usr/share/xml/scap/ssg/content/ssg-rl9-ds.xml
```

* Automatically attempt to remediate compliant issues
  
```shell
sudo oscap xccdf eval --profile xccdf_org.ssgproject.content_profile_stig --remediate \
/usr/share/xml/scap/ssg/content/ssg-rl9-ds.xml
```

* Generate a Checklist for STIG Viewer

```shell
oscap xccdf generate guide --profile xccdf_org.ssgproject.content_profile_stig --output stig-checklist.xml \
/usr/share/xml/scap/ssg/content/ssg-rl9-ds.xml
```

Now do it with your current results on system

```shell
sudo oscap xccdf generate guide \
--results stig-results.xml \
--output stig-checklist.xml \
/usr/share/xml/scap/ssg/content/ssg-rl9-ds.xml
```

* Generate an Ansible Playbook for Remediations
  
```shell
# `--fix-type ansible` generate ansbile playbook, for bash use `--fix-type` bash
# `results.xml` vulnerability scan result arf file
# `remediate-playbook.yml` output ansbile playbook file
sudo oscap xccdf generate fix \
  --fetch-remote-resources \
  --fix-type ansible \
  --result-id "" \
  results.xml > remediate-playbook.yml
```

## cscc

* Find your specific system version to begin using cscc/scap tools
  
[Security Content Automation Protocol (SCAP) – DoD Cyber Exchange](https://public.cyber.mil/stigs/scap/)

```bash
/opt/scc/cscc --config
```

Running a can within the ncurses screen:
1. Select 1.
	1. Select your specific system(s)
	2. Enter '0' to return back
2. Select 6
	1. Enter 1 to Scan regardless of any mismatch
	2. Select 0 to return
3. Select 0 to return
4. Select 9 to run scan and save scan test