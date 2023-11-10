# Alfred AWS Console Granted 

![demo](./.github/demo.gif)

Alfred Workflow to use [granted](granted.dev) to quickly open multiple AWS Console in Browser.
- Open Different AWS Accounts in Browser at the same time.
- Open Directly to a specific AWS Service.
- Open to a specific AWS Region.
- Prompt for Login/MFA if needed.

This workflow is a wrapper around [granted](granted.dev) CLI tool. Most of it's come from Granted itself. Thanks for the amazing tool.

# Prerequisites
- [granted](granted.dev) installed and configured.
  1. Install Granted
      ```bash
      brew tap common-fate/granted
      brew install granted
      ```
  2.  Follow Granted's [Getting Started](https://docs.commonfate.io/granted/getting-started/)


# Usage
1. Open Alfred and type `aws` to see the list of AWS Accounts you have access to.
   1. Use ⌘ to open to Console without selecting a service. 
   2. Use ⎇ to open to a specific region (By default granted uses Profile's region).

# License
[MIT License](./LICENSE)
Copyright (c) 2023 Sherif Abdel-Naby

# Credits
- [granted](granted.dev) for the Amazing CLI tool to manage AWS Console Access.
- Inspired by [aws-vault-alfred-workflow](https://github.com/kangaechu/aws-vault-alfred-workflow)


# Contribution

PR(s) are Open and Welcomed.
