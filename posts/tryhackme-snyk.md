---
title: "TryHackMe — Snyk"
description: "This module introduces the fundamentals of using Snyk to enhance software security. "
date: "2024-06-25"
headerImage: "https://miro.medium.com/v2/resize:fit:720/format:webp/0*P-rxSlVwV9sUaOQy"
tags: ["TryHackMe"]
---

# Introduction
__URL:__ https://tryhackme.com/module/snyk
Secure codes and dependencies. Learn to detect, evaluate and remediate vulnerabilities with Snyk.
This module introduces the fundamentals of using Snyk to enhance software security. You will learn to identify and understand security risks in open-source dependencies and codebases, effectively detect vulnerabilities, and apply remediation measures. The module also covers integrating security testing throughout the software development lifecycle and measuring the impact of security tools to ensure a robust security posture for your projects.

# Snyk Open Source
Securing open-source dependencies with Snyk - a junior application security engineer's journey.

## Open Source security risks
- **Large attack surface:** An application can use hundreds or thousands of frameworks and libraries. Hence, it requires a great amount of resources to manage each of these dependencies, which can sometimes be overwhelming. One package/library may introduce a small vulnerability, but it may severely affect the applicaions.
- **Fast-paced release cycles:** Because of bugs and securities, open-source libraries/frameworks frequently push out patches and updates. Falling behind on patching can expose our applications to security risks.
- **Transitive dependencies:** A lot of libraries/frameworks depend on others. When left unmanaged, transitive dependencies can harbor unknown vulnerabilites propagating throughout an entire system.
- **Limited visibility:** When struggling to keep track of the open-source components, developers may overlook some components and fail to detect vulnerability. Teams may also do not notice that they're using outdated versions, which may pose security issues.

## Diving Deeper Into Vulnerablities
- Prototype Pollution: Injecting malicious codes into Java Script object prototype's properties.

```
    // Assuming global variable `Object.prototype.__proto__ = {}` exists
    const pollutedObj = {
__proto__: {
  // Adding a new property to Array.prototype
  length: 999,
},
      };
    const array = [];
    array.__proto__ = pollutedObj;
    console.log(Array.length); // Output: 999 instead of expected value
```

## Vulnerability Triage
- Apply CVSS: calculate the severity of the vulnerabilites using CVSS (Common Vulnerability Scoring Systems)
- Consider the context: Not every vulnerability has high risk to a company. Other factors should be taken into considerations (financial loss, data sensitivity, compliance and policies, etc.)
- Evaluate exploitability: How likely we can perform the exploit also plays a role in evaluating the vulnerability.
- Collaboration across disiplines: communications with other deparments fosters vulnerabilities triage process, helping the team to determine priorities.

## Addressing vulnerabilites
Once vulernerabilites are identified, we can use the following approach:
- Dedicated backlog: Establish a separate list of high-priority vulnerabilites requiring immediate attentions -> easy to track progress and manage vulnerabilites.
- Break down tasks into smaller chunks: Break the fixes into smaller chunks so it can be easier to test and reducing the change of introducing side effects.
- Implement CI/CD pipeline: Automate build, unit test and integrating testing processes. 
- Perform thorough manual testing: Manual testings covers edge cases, which helps us identify vulnerabilies from different angles. These manual tests should be run in Staging environment before carrying out to production environment.
- Monitoring: Continue to monitor the applications and their performances after deployment. Some patches may bring some side effects/slow down in performances.

-----
# ANSWER THE QUESTIONS
- Which JSON-formatted manifest file serves as the central hub for Node.js projects, listing metadata, scripts, and dependency declarations?

-> pakage.json

- How many dependencies do we have for this new feature?

-> 5

- Which term describes indirect pakace dependencies formed through shared prerequisites, possibly concealing vulnerabilities and demanind cautiouos assessment?

-> transitive dependencies

- What single authentication mechanism allows users to transition smoothly amongst various linked platforms and services?

-> single sign-on

- What is the version of teh vulnerable lodash package?

-> 2.4.2

- Which vulnerability allows an attacker to modify an Object?

-> prototype pollution

- What does CVSS stand for?

-> Common Vulnerability Scoring System

- Should the development team bulk fix all the vulnerabilities found in this new feature? (y/n)

-> n

- How does CircleCI help streamline pipeline configuration and standardization?

-> orb

What file defines the GitHub Actions workflow configuration that enables automation and customized sequences for building, testing and deploying?

-> yaml

- Which collaborative DevOps practice combines real-time communication channels, automation and operational agility?

-> Chatops

# Snyk Code
Securing code with Snyk - a junior application security engineer's journey.

## Code Security Risks
- **Complex codebases:** With applications serving business needs, the codebases sometimes can be huge and complex, which can obscure the vulnerabilities, making them harder to detect and remediate.
- **Insufficient security practices:** A lot of developers prioritize functionalities over security in their codes, especially in high-pressure environments. This can result in insufficient coding practices and standards, making the applications vulnerabile to security risks.
= **Limited external scrutiny:** Codebases within a company do not have the "many eyes" principle that supports open-source security, it is easier to have security issues go unoticed for a long period of time, increasing the risk of Zero-day vulnerability.
- **Dependency on third-party components:** It is common that companies use third-party libraries/frameworks, which can introduce vulnerabilites.
- **INternal knowledge gaps:** Team may lack expertise in secure coding practices or unaware of the latest security threats and mitigation strategies.

## Remediating Vulnerabilites
### AI-Generated Code Scrutiny
AI-Generated codes can introduce security risks. Hence it is important to audit these codes:
- Manual review: encourage manual inspection of all AI-generated code, focusing on security-sensitive sections
- Pair programming: implement pair programming for AI-generated code sections, combining AI efficiency with human scrutiniy insight.
- Security testing integration: Integrate automated security testing tool like SnykCOde, ensuring immedate feedback on potential vulnerabilities introduced by AI.

### Strategic Vulnerability Prioritization
- Business logic consideration: Prioritize vulnerabilities directly affecting business operations/exposing business data
- Feature-focus enhancement: Some features of the applications are more important than others. Hence, it is important to prioritize vulnerabilites affecting those features.

## Establish Best Practices
### Refine Security Policies for propriety and AI-Generated code
- With the growing of AI, we should have policy to include AI-generated code segments in security testings. 
- There should also be guideline for secure coding practices when it comes to AI-generated code.

### Enhance Collaborations Across Teams
- Use [OWASP Security Champions Playbook](https://github.com/c0rdis/security-champions-playbook) to establish cross-functional security forumes where developers, security team members and AI specialists can share insights, discuss new security trends and collaborate on secure coding practices.

### Measure the impact of Security Tool implementation
- When it comes to implementing Security Tools, there are a great number of factors to consider. Use the following frameworks as a guideline:
	- Cost reduction: Calculate the costs associated with security before vs after the implementation (remediation, fines, downtime, etc.)
	- Efficiency gains: the time saved by developers and security team in identifying & addressing vulnerabilities through automated scans and integrations. 
	- Preventive savings: Estimate the costs of potential security breaches that were avoided due to proactive vulnerability management. 
	- ROI formula: use the following formula to calculate ROI

   		![image](https://github.com/hhphu/InfoSec/assets/45286750/49c292f7-074d-43ff-bef6-a282ea43a588)

## ANSWER THE QUESTIONS
- How many vulnerabilities are flagged on the search-feature.js file?

-> 4

- How many high-severity vulnerabilities are flagged on the search-feature.js file?

-> 2

- What are the two medium-severity vulnerabilities flagged on the search-feature.js file? (in alphabetical order)

-> Cross-Site Request Forgery, Information Exposures

- What is the CWE for Cross-Site Scripting?

-> CWE-79

- What is the CWE for SQL Injection?

-> CWE-89

- WHat is the unsnitised user input in the chat-controller.js file?

-> searchTerm

- What is the new vulnerability introduced with the XSS fix?

-> Allocation of resources without limits or throttling

- Which Express method is used to fix the XSS vulnerability in the code snippet?

-> res.render

- What is the updated code in the code snippet using to fix the SQL injection?

-> Parameterised queries

- Establishing sensible alert thresholds in continous monitoring practices involves considering the severity, frequency, and rate of change of vulnerabilites (y/n)

-> OWASP Security Champions Playbook

