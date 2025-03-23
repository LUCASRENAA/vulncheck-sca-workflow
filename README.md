# Vulncheck SCA Workflow

A reusable GitHub Actions workflow for scanning Python dependencies for security vulnerabilities using Software Composition Analysis (SCA).

## Overview

This project provides a GitHub Actions workflow that automatically scans Python project dependencies for known security vulnerabilities. It analyzes your `requirements.txt` file and generates a detailed vulnerability report.

## Features

- Automated dependency scanning
- Detailed vulnerability reports in Markdown format
- Support for Python projects
- Configurable requirements file path
- Color-coded severity levels in reports
- CVSS score tracking
- Automated fail/pass based on vulnerability detection

## Usage

1. Add the workflow to your repository by creating `.github/workflows/run-sca.yml`:

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
      requirements_file: 'requirements.txt'
      technology: 'python'
      api_url: 'http://95.111.232.70/api/scans/'
