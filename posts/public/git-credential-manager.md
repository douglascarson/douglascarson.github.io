# Git Credential Manager
## Background
When authenticating with Azure DevOps Repos there is three ways to authenticate:
* SSH (Public and private key)
* PAT (Personal Access Token)
* HTTPS authentication using Git Credential Manager

## Setting up Git credential Manager
To set up Git credetial manager you can install it as part of the Git Installation wizard.
Once it is configured and you clone a repo, you will be prompted for you login credentials to ADO.

Once you have successfully authenicated the credentials this will do two things:

1. A PAT token will get created within ADO
2. The credential will be stored on the local machine windows credential store under generic credentials.

To see which credential manager git is configured to use you can run the below commands:

```
git config --global credential.helper
git config --system credential.helper
git config --local credential.helper

``` 
When using windows git credential manager it will show
```
git config --system credential.helper
manager
```
To edit the git config you can run the command
```
git config --edit --system
```