# ftp-delete-action
Automate deleting files on your ftp server using this GitHub action.

Useful for cleaning up logs/tempfiles or removing files that are autogenerated with a new name for each build. For example `precache-manifest.*.js` in a ReactJS build.  


This action is inspired by https://github.com/sebastianpopp/ftp-action which is used to automate the ftp copy process.  

The example below uses GitHub secrets to generate the parameters you don't want to have visible in your repo: https://docs.github.com/en/actions/configuring-and-managing-workflows/creating-and-storing-encrypted-secrets  

## Example usage

```
name: Deploy via ftp
on: push
jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout repo 
      uses: actions/checkout@v2
      
#   ... INSTALL / RESTORE / BUILD ...  

    - name: Clean ReactJS precache-manifest and logs
      uses: StephanThierry/ftp-delete-action@v1
      with:
        host: ${{ secrets.FTP_SERVER }}
        user: ${{ secrets.FTP_USERNAME }}
        password: ${{ secrets.FTP_PASSWORD }}
        remoteFiles: "precache-manifest.*.js;logs/*.log"
        remoteDirectories: "/App_Data/TEMP"
        workingDir: "/public_html"
        ignoreSSL: "1"

#   ... THE REST OF YOUR DEPLOYMENT ...  

```

## Input parameters

Input parameter | Description | Required | Example
--- | --- | --- | ---
host | FTP server name | Yes | ftp.domain.com
user | FTP username | Yes | ftpUser
password | FTP password | Yes | secureFtpPassword
remoteFiles | Files to delete separated by ";" | Yes | `precache-manifest.*.js;logs/*.log`
remoteDirectories | Directories to delete separated by ";" | No | `/App_Data/TEMP`
workingDir | Working directory (Use "." if you want the server default and not "/") | No, default=`/` | `/public_html`
ignoreSSL | Ignore invalid TLS/SSL certificate (1=ignore)  | No, default=0 | 1
