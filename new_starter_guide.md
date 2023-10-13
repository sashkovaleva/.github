# New Starter Set Up Guide
Welcome to the team! 😃

## What we Use
- **Azure DevOps** - you should receive an invitation to join via email
- **GitHub** - you should receive an invitation to join via email

## Service Now Requests
You will need to raise ServiceNow requests for the following:
- [BGE SMDS SUPPORT](https://cnprod.service-now.com/sys_user_group.do?sys_id=40698852dbaea4102c8ee7ba48961928&sysparm_view=text_search)
-	[CUSTOMER FORWARD DATA STORE SUPPORT](https://cnprod.service-now.com/sys_user_group.do?sys_id=d8c3b3bf1b7c5950f5ed0e92f54bcb7f&sysparm_view=text_search)
-	[MAAS INTEGRATION SUPPORT](https://cnprod.service-now.com/sys_user_group.do?sys_id=6643c0161b528514953d8485d34bcb5f&sysparm_view=text_search)
-	[Integration Engineering](https://cnprod.service-now.com/sys_user_group.do?sys_id=afe579a11b5e851082a88485d34bcb83&sysparm_view=text_search)


## Required Software Installation
### Homebrew (Mac only)
Run the below command to install homebrew on the system
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```
---
### Install NVM (Node Version Manager)

1. Use homebrew to install nvm on the system
```
brew install nvm
```

3.	Create nvm directory in the home folder
```
mkdir -p ~/.nvm
```

5. Add the below lines to ~/.bash_profile ( or ~/.zshrc for macOS Catalina or newer versions)
```
  export NVM_DIR=~/.nvm

  source $(brew --prefix nvm)/nvm.sh
```
4.	Install the latest version of node with 
```
nvm install node
```
*Note: Please ensure that the node version you are installing as default is 16*

---
### AWS CLI (Latest Version)

1.	Go to https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
2.	Select the appropriate OS and continue with installation steps mentioned.
3.	After installing, open command prompt or terminal and type in command `aws --version` to confirm installation
---
### Git
1.	Go to https://git-scm.com/book/en/v2/Getting-Started-Installing-Git
2.	Follow the instructions provided for the appropriate OS required.
3.	Verify git installation by typing the command `git --version` 

---
### Install TFENV to switch between different versions of terraform (Mac only)

Install TfENV to switch between versions of terraform as different repositories are using different versions
1.	Go to https://github.com/tfutils/tfenv 
2.	Follow the instructions provided for manual installation

**For Mac**
```
brew install tfenv
```

1.	Install the required version of terraform
```
tfenv install <version>
```
3.	To use/switch specified version
```
tfenv use <version>
```
5.	To list all the versions installed
```
tfenv list.
```

*Note: When using terraform on mac, Please ensure to set the environment variable TFENV_ARCH to amd64.
You can run the below command in your terminal before running terraform. Or, the same command can be pasted into the .bashrc file to auto load the property in every terminal
`export TFENV_ARCH = amd64`*

---

### Code editor – VS Code
1.	Go to https://code.visualstudio.com/download
2.	Select the appropriate OS and download the installer and install.
3.	In windows – After clicking install , check the box ‘open with vscode’ and continue. This will help to open a folder in vscode directly by right clicking and selecting open with vscode. 
4.	Another way is to open the folder in command prompt(cmd) and type ‘code .’ and enter to open the folder or file in vscode.
5.	Alternate ways in mac – Open Centrica app store and find vscode. Click install and continue.
6.	Follow the below link to add a quick action in mac to get ‘open in vscode’ option for folders and files.
 https://www.jimbobbennett.io/open-anything-in-vs-code-using-a-macos-quick-action/

---
### Iterm2 for Mac - Optional
1.	Open Centrica app store and find ‘iterm2’.
2.	Click on install and continue. Restart might be required.

---
### Docker for Desktop - Optional
1.	Go to https://www.docker.com/products/docker-desktop
2.	Select the OS and download the installer.
3.	Please follow the steps in below link to install Docker.
https://docs.docker.com/desktop/mac/install/
4.	Will add steps after I install it. – licencing implication to figure out.

---
### IDE IntelliJ Ultimate Edition
1.	Go to https://www.jetbrains.com/idea/download/#section=mac
2.	Select the OS and download the installer.
3.	User gets 1 month of free Ultimate edition with the install, if you need licensing for this, reach out to Titu.

---
### SDKManager (Mac)
Install SDK manager to switch between different versions on Java and maven so that SDK upgrades can be possible
Steps
1.	Run the below commands in order
```
curl -s "https://get.sdkman.io" | bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
```
2.	Run the below command to see the list of all JAVA SDKs available for download and install
```
sdk list java
```
4.	Run the below command to pick a version of java and install it
```
sdk install java <java version from list>
```

---

### Setup Accounts on your machine using AWS-ADFS
**Pre-requisites**
1.	Pip OR pipx must be installed on the system
**Steps to install**
1.	Install aws-adfs
```
pip install aws-adfs
```
2.	Use the web login portal to find out the account names for the aws profiles that have been assigned to you
3.	Use aws-adfs to login with the account on AWS CLI
```
aws-adfs login --adfs-host=sts.centricaplc.com --role-arn arn:aws:iam::455642533373:role/Prod-UKH-Mode2DigitalK8sDevOps --provider-id urn:amazon:webservices
```
4.	Once the login is successful, Open the ~/.aws/config file to view the entries added for the logged in account
5.	Duplicate the entries with a new profile name for as many AWS accounts you have access to.
The entries should look as below
```
[profile preprodServicesBGE]
region = eu-west-2
output = json
adfs_config.ssl_verification = True
adfs_config.role_arn = arn:aws:iam::<ACCOUNT_ID>:role/<Role>
adfs_config.adfs_host = sts.centricaplc.com
adfs_config.session_duration = 3600
adfs_config.provider_id = urn:amazon:webservices
adfs_config.sspi = False
adfs_config.duo_factor = None
adfs_config.duo_device = None
adfs_config.u2f_trigger_default = True
```
7.	Login to the aws accounts with
```
aws-adfs login --profile <profile name (in example preprodServicesBGE)>
```

---

### Cloning repositories and creating Terraform workspaces for dev testing
**Clone the repositories**

1.	Create a new folder to set up the code. Open terminal. Copy the https(grs) link of the repository and run the below command.
2.	You need to add ‘profile name’ in the clone url of the repository after the ‘//’ and add an ‘@’ after the profile name. Similar to this
`codecommit::eu-west-1://adfs@smds-process-landing-files`
5.	run the command 
```
git clone codecommit::eu-west-1://adfs@smds-process-landing-files
```
It will be prompted for password and enter the password of your account.
5.	You have successfully cloned the repo.

---

### Steps to create Terraform workspace
1.	Open the smds-infrastructure in terminal and type in 
```
export AWS_PROFILE = “tools account profile”
export terraform.workspace = “Name of the workspace”
```
2.	Now run the below commands
```
Terraform init
Terraform plan
Terraform apply
```
3.	Your workspace will be created in DevTest account.

---

### GitHub
1.	Get the invite to the GitHub account from the SRE team. Create your account with your centrica email
2.	Talk to someone on the team to get yourself added to the integration Team
3.	Create a new ssh key for github 
`ssh-keygen -t ed25519 -C your_email@example.com`
4.	Register key in GitHub under Settings -> SSH and GPG keys

Detailed steps are listed [here](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)

*Note:
If you have multiple github accounts using different SSH keys, you will need to setup sshconfig to specify which key to use on which account.*


