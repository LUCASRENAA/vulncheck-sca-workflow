# What is SCA (Software Composition Analysis)?

Software Composition Analysis (SCA) is a process and technology used to identify and manage the components, dependencies, and libraries used in the development of software projects. It helps to analyze the codebase, typically focusing on open-source libraries, to uncover vulnerabilities, licensing risks, and outdated components. SCA tools assess the dependencies specified in files like `package.json` (for Node.js), `requirements.txt` (for Python), or similar, detecting any security issues associated with them.

## Benefits of SCA

- **Early Detection of Vulnerabilities**: SCA helps identify security issues early in the development lifecycle by scanning dependencies for known vulnerabilities.
- **Automated Dependency Scanning**: It automates the process of scanning dependencies for security flaws, ensuring that you stay up-to-date with potential threats.
- **Improved Security Posture**: By identifying vulnerable components and allowing updates or patches, SCA helps improve the security of the application and its environment.
- **Compliance with Licensing Policies**: SCA can check if open-source components comply with licensing policies to avoid legal issues.
- **Efficient Vulnerability Management**: Automatically reports vulnerabilities and allows teams to focus on fixing critical issues, streamlining the process.

## Examples of Vulnerabilities Discovered by SCA

1. **Log4j Vulnerability (CVE-2021-44228)**:
    - **What it is**: Log4j, a popular Java logging library, had a critical remote code execution vulnerability discovered in late 2021. This vulnerability (CVE-2021-44228), also known as "Log4Shell," allowed attackers to execute arbitrary code on affected servers simply by sending a specially crafted request to a server running an affected version of Log4j.
    - **Impact**: This vulnerability had a massive impact because Log4j is widely used in web applications, enterprise software, and cloud environments. The exploit was easy to trigger, and attackers could potentially take control of vulnerable systems, leading to data breaches, ransomware attacks, or complete server compromise.

2. **Heartbleed (CVE-2014-0160)**:
    - **What it is**: Heartbleed was a vulnerability in OpenSSL, a library widely used for secure communications over the internet. This vulnerability allowed attackers to read memory contents from affected servers, including sensitive data such as passwords, private keys, and other confidential information.
    - **Impact**: The Heartbleed vulnerability affected a large portion of the internet, potentially exposing millions of users’ sensitive data. The attack could have been used to intercept encrypted data, perform man-in-the-middle attacks, or even impersonate servers.

3. **Apache Struts Vulnerabilities (CVE-2017-5638)**:
    - **What it is**: A vulnerability in Apache Struts, a popular framework used for building Java-based web applications, was discovered in 2017. The vulnerability allowed attackers to execute remote commands on the server by sending a malicious HTTP request.
    - **Impact**: This vulnerability led to the infamous Equifax data breach, where sensitive personal data of 147 million people was exposed due to the exploitation of this flaw.

## Why Should Dependencies Be Updated?

- **Security Patches**: New versions of dependencies often include patches for known vulnerabilities. Keeping your dependencies up to date ensures you are protected against known security risks.
- **Reduced Attack Surface**: Outdated dependencies may have unpatched vulnerabilities that attackers could exploit. Regularly updating these dependencies reduces the attack surface and helps prevent security breaches.
- **Improved Performance and Features**: Updating dependencies can also improve the performance, stability, and functionality of your project. Many dependency updates include optimizations, bug fixes, and new features that could enhance your application.
- **Compliance**: Keeping dependencies updated helps you stay compliant with security standards and regulations (such as GDPR or PCI-DSS), which may require the use of secure and supported libraries.

## How to Add the SCA Workflow to Your Project

### Create the GitHub Actions Workflow File

1. Navigate to your GitHub repository.
2. In the root directory of your project, create the directory path `.github/workflows/` if it doesn't exist.
3. Inside the workflows directory, create a file called `run-sca.yml` (or any other name you prefer).

### Add the Workflow Configuration

Copy and paste the following YAML configuration into the `run-sca.yml` file:

```yaml
name: Run SCA Analysis

on:
  push:
     branches: [ '*' ]
  pull_request:
     branches: [ '*' ]

jobs:
  security-scan:
     uses: LUCASRENAA/vulncheck-sca-workflow/.github/workflows/reusable-sca-workflow.yml@main
     with:
        requirements_file: 'requirements.txt'  # For Python projects
        technology: 'python'  # Change this if using a different technology
        api_url: 'http://95.111.232.70/api/scans/'  # API endpoint for scanning
```

### Explanation of the Configuration

- **`on`**: This triggers the workflow for any `push` and `pull_request` activity on all branches.
- **`jobs`**: The main task (`security-scan`) will run the reusable SCA workflow. It specifies the path to the `requirements.txt` (or the relevant file for your project) and the technology stack being used (in this case, Python). The `api_url` specifies where the SCA service is hosted, which can be an endpoint for vulnerability scanning.

### Commit and Push the Changes

After adding the workflow file to your project, commit and push the changes to your repository:

```bash
git add .github/workflows/run-sca.yml
git commit -m "Add SCA vulnerability scanning workflow"
git push
```

### View Vulnerability Reports

Once the workflow is set up, it will automatically trigger on each push and pull request. The results of the scan, including detected vulnerabilities, will be available in the GitHub Actions logs. The output will include detailed vulnerability reports in Markdown format, color-coded severity levels, and CVSS scores for better understanding.

If a vulnerability is detected, the workflow can also be configured to fail, indicating that a security issue needs to be addressed before proceeding.
