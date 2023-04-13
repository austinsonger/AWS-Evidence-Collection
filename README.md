# Github Action for AWS Evidence Collection

> When you use this. The project should be private for obvious reasons.



#### Evidence That will be collected:


| Evidence                                                     | AWS Command | Subcommand              | SOC2 Mapping               | NIST Mapping |
| :----------------------------------------------------------- | ----------- | ----------------------- | :------------------------- | ------------ |
| Inbound network access to server management ports is controlled and restricted to defined sources |             |                         | CC6.1, CC6.3, CC6.6, CC6.7 |              |
| The default configuration of server network access is restricted and denies all access |             |                         | CC6.1, CC6.3, CC6.6, CC6.7 |              |
| Buckets have delete protection enabled to protect bucket deletion |             |                         | CC6.1, CC6.3, CC6.6, CC6.7 |              |
| Public access to buckets is restricted to prevent uncontrolled or unauthorized access |             |                         | CC6.1, CC6.3, CC6.6, CC6.7 |              |
| Network access to buckets is restricted to disallow non-conforming traffic and protocols such as HTTP |             |                         | CC6.1, CC6.3, CC6.6, CC6.7 |              |
| Inbound network access to management ports is controlled and restricted to defined sources |             |                         | CC6.1, CC6.3, CC6.6, CC6.7 |              |
| Web Application Firewall (WAF) rules are configured to protect network access |             |                         | CC6.1                      |              |
| Load balancers have only HTTPS/SSL listeners to enforce encryption in transit | `elbv2`     | `describe-ssl-policies` | CC6.1, CC6.3, CC6.6, CC6.7 |              |
| Load balancers have delete protection enabled to protect from deletion |             |                         | CC6.1, CC6.3, CC6.6, CC6.7 |              |
| Database backups are encrypted to prevent unauthorized access to information |             |                         | CC6.7                      |              |
| Encryption configuration for databases used in production environments |             |                         | CC6.7                      |              |
| Encryption configuration for server disks used in production environments |             |                         | CC6.7                      |              |
| Server disk backups are encrypted to prevent unauthorized access to information |             |                         | CC6.7                      |              |
| Encryption configuration for buckets used in production environments |             |                         | CC6.7                      |              |
| Encryption keys are rotated frequently to mitigate the risk of unauthorized access to cryptographic keys |             |                         | CC1.1                      |              |
| Listings of access keys, secret keys, API keys and other cryptographic keys stored on the key management server |             |                         |                            |              |
| TLS certificate of the production application proving confidential traffic. |             |                         |                            |              |
|                                                              |             |                         |                            |              |
|                                                              |             |                         |                            |              |
|                                                              |             |                         |                            |              |





#### Current Setup

```
name: AWS Evidence Collection

on:
  push:
    branches:
      - main
  schedule:
    # Runs "At 12:00 on day-of-month 28."
    - cron: '0 12 28 * *'

permissions:
  contents: write

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v2
        with:
         aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
         aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
         aws-region: us-east-1
         
      - id: install-aws-cli
        uses: unfor19/install-aws-cli-action@v1
        with:
         version: 2     # default
         verbose: false # default
         arch: amd64    # allowed values: amd64, arm64
         rootdir: ""    # defaults to "PWD"
         workdir: ""    # defaults to "PWD/unfor19-awscli"
            
      - uses: GuillaumeFalourd/command-output-file-action@v1
        with:
          command_line: aws acm-pca list-certificate-authorities --output json
          output_file_name: Lists/certificates.json
          display_file_content: YES #   
        
################################################################ 
# TLS certificate of the production application proving confidential traffic.
#      - uses: GuillaumeFalourd/command-output-file-action@v1
#        with:
#         command_line: 
#         output_file_name: 
#         display_file_content: YES # 
################################################################
# Load balancers have only HTTPS/SSL listeners to enforce encryption in transit
#     - uses: GuillaumeFalourd/command-output-file-action@v1
#       with:
#         command_line: aws elbv2 describe-ssl-policies --output json
#         output_file_name: Configurations/load-bal-ssl-config.json
#         display_file_content: YES #  
################################################################
# Inbound network access to server management ports is controlled and restricted to defined sources
#      - uses: GuillaumeFalourd/command-output-file-action@v1
#        with:
#         command_line: 
#         output_file_name: 
#         display_file_content: YES # 
################################################################
# The default configuration of server network access is restricted and denies all access
#      - uses: GuillaumeFalourd/command-output-file-action@v1
#        with:
#         command_line: 
#         output_file_name: 
#         display_file_content: YES # 
################################################################
# Buckets have delete protection enabled to protect bucket deletion
#      - uses: GuillaumeFalourd/command-output-file-action@v1
#        with:
#         command_line: 
#         output_file_name: 
#         display_file_content: YES # 
################################################################
# Public access to buckets is restricted to prevent uncontrolled or unauthorized access
#      - uses: GuillaumeFalourd/command-output-file-action@v1
#        with:
#         command_line: 
#         output_file_name: 
#         display_file_content: YES # 
################################################################
#  Network access to buckets is restricted to disallow non-conforming traffic and protocols such as HTTP
#      - uses: GuillaumeFalourd/command-output-file-action@v1
#        with:
#         command_line: 
#         output_file_name: 
#         display_file_content: YES # 
################################################################
#  Web Application Firewall (WAF) rules are configured to protect network access
#      - uses: GuillaumeFalourd/command-output-file-action@v1
#        with:
#         command_line: 
#         output_file_name: 
#         display_file_content: YES # 
################################################################
# Load balancers have delete protection enabled to protect from deletion
#      - uses: GuillaumeFalourd/command-output-file-action@v1
#        with:
#         command_line: 
#         output_file_name: 
#         display_file_content: YES # 
################################################################
# Database backups are encrypted to prevent unauthorized access to information
#      - uses: GuillaumeFalourd/command-output-file-action@v1
#        with:
#         command_line: 
#         output_file_name: 
#         display_file_content: YES # 
################################################################
# Encryption configuration for databases used in production environments
#      - uses: GuillaumeFalourd/command-output-file-action@v1
#        with:
#         command_line: 
#         output_file_name: 
#         display_file_content: YES # 
################################################################
# Encryption configuration for server disks used in production environments
#      - uses: GuillaumeFalourd/command-output-file-action@v1
#        with:
#         command_line: 
#         output_file_name: 
#         display_file_content: YES # 
################################################################
# Server disk backups are encrypted to prevent unauthorized access to information
#      - uses: GuillaumeFalourd/command-output-file-action@v1
#        with:
#         command_line: 
#         output_file_name: 
#         display_file_content: YES # 
################################################################
# Encryption configuration for buckets used in production environments
#      - uses: GuillaumeFalourd/command-output-file-action@v1
#        with:
#         command_line: 
#         output_file_name: 
#         display_file_content: YES # 
################################################################
# Encryption keys are rotated frequently to mitigate the risk of unauthorized access to cryptographic keys
#      - uses: GuillaumeFalourd/command-output-file-action@v1
#        with:
#         command_line: 
#         output_file_name: 
#         display_file_content: YES # 
################################################################
# LIST OF ACCESS KEYS        
      - uses: GuillaumeFalourd/command-output-file-action@v1
        with:
         command_line: aws iam list-access-keys --output yaml
         output_file_name: Lists/access-keys.yaml
         display_file_content: YES #  
################################################################

      - name: Commit & Push changes
        uses: actions-js/push@master
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}

```












