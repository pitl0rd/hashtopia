
# Cloud Provider Processing Examples

These examples demonstrate how cloud-service credentials may be processed during **authorized security assessments**, audits, or incident response investigations.  
All actions described here **must only be performed with explicit permission** and within the scope of an approved engagement.

---

## AWS (Amazon Web Services)

### Scenario

During an authorized assessment, you are provided with an **AWS access key**, **secret key**, and optionally a **.pem key** for a user with defined permissions.  
These credentials allow you to enumerate accessible AWS resources to evaluate configuration, privilege boundaries, and overall security posture.

---

### Step 1: Gather Tools

Clone and install the Pacu AWS auditing framework:

[https://github.com/RhinoSecurityLabs/pacu.git](https://github.com/RhinoSecurityLabs/pacu.git)

---

### Step 2: Launch Pacu

`python3 pacu.py`

---

### Step 3: Configure AWS Credentials

Within Pacu, configure the provided credentials:

- **Key Alias** - Internal identifier used by Pacu only
    
- **Access Key** - AWS access key ID for the user
    
- **Secret Key** - Secret associated with the access key
    
- **Session Token (optional)** - Temporary credential if STS is in use
    

---

### Step 4: Enumerate AWS Resources

List available modules or execute an enumeration module:

`> ls > run enum_ec2`

---

### References

- [https://github.com/RhinoSecurityLabs/pacu/wiki](https://github.com/RhinoSecurityLabs/pacu/wiki)
- [https://github.com/carnal0wnage/weirdAAL](https://github.com/carnal0wnage/weirdAAL)
- [https://github.com/toniblyx/my-arsenal-of-aws-security-tools](https://github.com/toniblyx/my-arsenal-of-aws-security-tools)

---

## Microsoft Azure

### Scenario

As part of an approved assessment, access is granted to a **privileged Azure AD user** (such as Owner or Contributor).  
Analysis focuses on identifying what secrets, credentials, and configurations the account can access, including:

- Key Vaults
    
- App Service configurations
    
- Automation Accounts
    
- Storage Accounts
    

---

### Step 1: Gather Tools

Install the required PowerShell modules:

`Install-Module -Name AzureRM Install-Module -Name Azure`

Download MicroBurst:

[https://github.com/NetSPI/MicroBurst](https://github.com/NetSPI/MicroBurst)

Import the credential enumeration module:

`Import-Module .\Get-AzurePasswords.ps1`

---

### Step 2: Enumerate Accessible Secrets

Execute the module to enumerate credentials across Azure services.  
You will be prompted for account and subscription details.

`Get-AzurePasswords -Verbose | Export-CSV azure_credentials.csv`

---

### References

- [https://blog.netspi.com/get-azurepasswords/](https://blog.netspi.com/get-azurepasswords/)
    

[https://nostarch.com/azure](https://nostarch.com/azure)

---

## GCP (Google Cloud Platform)

### Step 1: Install the gcloud CLI

Follow Google’s installation instructions:

[https://cloud.google.com/pubsub/docs/quickstart-cli](https://cloud.google.com/pubsub/docs/quickstart-cli)

---

### Step 2: Configure Authorized Credentials

`gcloud config set account <account>`

---

### Step 3: Run ScoutSuite

Using a user account:

`python Scout.py --provider gcp --user-account`

Using a service account:

`python Scout.py --provider gcp --service-account --key-file /path/to/keyfile`

---

### Step 4: Define Enumeration Scope

Specify one of the following targets:

- **Organization** - `organization-id <ORGANIZATION_ID>`
    
- **Folder** - `folder-id <FOLDER_ID>`
    
- **Project** - `project-id <PROJECT_ID>`
    

---

### Reference

[https://github.com/nccgroup/ScoutSuite](https://github.com/nccgroup/ScoutSuite)

---
[[Processing]]
[[Home]]
#sudad #tools #howto 