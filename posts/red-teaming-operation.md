---
title: "Red Teaming Operation"
description: "A project that goes through modern penetration tester and read teamer methodologies, creating documentations and generating reports in a timely fashion."
date: "2023-07-12"
headerImage: "https://www.immuniweb.com/images/red-teaming-vs-penetration-testing/red-teaming.jpg"
tags: projects
---

## Targeting Organization Reconnaissance
- Perform stealthy reconnaissance against organizations to avoid potential tripwires and discover open services. The service may include websites and development end-points.
    - Searched for "Learn About Security" on Shodan or other recon websites and provide a screenshot for evidence of discovery.
    - Searched and provided evidence of the DNS zone information for learnaboutsecurity.com.
- Identify DNS Records for target organization.
    - Identify the company websites, email provider, and spam service (if one exists).
    - Provide a list of names to IP mappings for "Learn About Security" websites and hosting services.
- Identify the company websites, email provider, and spam service (if one exists).
    - Identify the Content Management System in use for the target organization.

## Scanning for Vulnerabilities
- Scanners like nmap will be used to map out the internal network.
- Scan the external web site and associated DNS server to identify vulnerable component(s).
- Scan the VM and identify vulnerable software.

## Vulnerability Research and Identification
- Confirm presence of sensitive data and gain access to stored credentials and sensitive data.
    - Research the VM’s remote access software in a few ways:
        - If credentials are stored in plain text?
        - If there are any major security weaknesses that are often overlooked?
        - If there are any software, platform, or version vulnerabilities that are exploitable?
- Research how to Review Old Backup and Unreferenced Files for Sensitive Information
    - Research GitHub source code vulnerabilities and Old Backup and Unreferenced Files for Sensitive Information.
        - Conduct a blind guessing using the organization naming scheme for published content
        - Filename filter bypass

## Exploiting Vulnerable Services
- Run Brute force attack against the Security Training Server (debianx64DMZOnCloudNew)
- Gain remote access to the Win-10 Machine by exploiting an application on the device
    - Utilize Metasploit Framework (exploit module) to complete the following:
        - Search for exploits relating to the application manufacturer (Xampp)
        - Configure the settings for the exploit
        - Change the payload to one that functions if the default payload is not successful.
        - Run the exploit to gain a remote shell into the target device
## Reporting
- Executive summary should be light on technical details. The findings section should describe the vulnerabilities in enough detail to be reproducible.
    - Discussions in the Executive Summary section should be policy and procedure oriented. Include recommendations for executives to improve the following:
        - Software maintenance and version policies
        - Sensitive data handling policies should be mentioned
        - Password policies may also be discussed by the student if they recovered the root password

- Prioritized the vulnerabilities found.
    - The vulnerabilities that were found are prioritized and give an appropriate reason and source for the importance based on the audience.
    - The student should provide some initial notes to reduce risk of attack against the most vulnerable services by tier or rank.
        - i.e. PII data leak, High Impact
        - Outdated versions, High Risk

- Methodology should be documented well enough to be reproducible.
    - Methodology should be reproducible.
        - Commands should be documented
        - Exploit code source locations should be discoverable.
        - General scientific method based flow of methodology:
            - The action resulted in the identification of some evidence-based discovery.
            - The discovery was researched and produced a probable finding.

- The report can be found [here](https://github.com/hhphu/images/blob/main/Red%20Team%20Operations/Red%20Team%20Operations%20Report.pdf)

---

### ACCOMPISHMENTS - LESSON LEARNED - SKILLS GAINED
- Deployed a secure network infrastructure using VirtualBox, including a Debian server in the DMZ, a web application server in the DMZ, an internal network device in the MZ, a public server, and two firewalls for network segregation.
- Employed a suite of reconnaissance tools like Wappalyzer, DNS scan, wpscan, dirb, and nmap, to conduct thorough assessments of target applications, enhancing understanding of potential attack vectors and weak points.
- Conducted thorough OSINT investigations utilizing diverse resources such as GitHub, job postings, phishing campaigns, and StackOverflow, augmenting the intelligence gathering process and enhancing overall assessment outcomes.
- Utilized industry-standard tools like Metasploit, BurpSuite, and exploitdb, to exploit systems, successfully gaining unauthorized access to DMZ servers and devices in the MZ, uncovering other critical vulnerabilities.
- Leveraged Hydra, John, and Hashcat to gain access to crack passwords to access systems and escalate privileges.
- Produced meticulous assessment reports, encompassing an executive summary, testing methodology, vulnerability scores and descriptions, exposure analysis, modified exploit code, and inclusive screenshots captured during the assessment. The report served as a comprehensive deliverable, providing stakeholders with actionable insights and recommendations for effective security improvements.

