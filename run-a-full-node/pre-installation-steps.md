# Installation Steps

There are two ways to obtain Onomy Binary

1. [Download it from our GitHub](pre-installation-steps.md#undefined)
2. [Use provided scripts to compile binary locally](pre-installation-steps.md#follow-these-steps-to-obtain-the-binaries)



#### 1. Download Binary from our GitHub

1. First make sure you have golang installed and $PATH is pointing to where go binary is installed. Also set the $GOPATH environment variable.

2\. head over to our \[releases page]\([https://github.com/onomyprotocol/onomy/releases](https://github.com/onomyprotocol/onomy/releases)) and download the latest version of the Onomy Binary

Default structure for Onomy files is as below

```
$HOME
  |- .onomy (Default home location for onomy chain)
      |- bin (Default location for all the binaries)
      |- scripts (Default location of all the scripts)
      |- src (Default location for source code, in case binaries are compiled locally)
```

3\. Copy the downloaded binary to `$HOME/.onomy/bin` directory and add executable permission `chmod +x onomyd`

4\. Add `$HOME/.onomy/bin` to $PATH environment variable. `export PATH=$PATH:$HOME/.onomy/bin`

5\. To make changes in PATH variable persistent, run the following command, which will add an export command to your `.bashrc` file. `echo "export PATH=$PATH`:$HOME/.onomy/bin`" >> ~/.bashrc`



#### 2. Use provided scripts to compile binary locally

1. Download the bin.sh script\
   \
   `wget https://raw.githubusercontent.com/onomyprotocol/onomy/dev/deploy/testnet/scripts/bin.sh`
2. Add the executable permission using `chmod +x bin.sh`
3. run the script using `bash bin.sh`
4. Restart terminal so changes made in environment can take effect

This script downloads and compiles Onomy binary. A new directory `.onomy` will be created in your home directory. All the source code will be downloaded in `.onomy/src` directory and compiled binary will be in `.onomy/bin` directory.

{% hint style="danger" %}
NOTE: Do not run the script as a super user. If you run script as a super user, then the .onomy directory will be created in home directory of super user. Script will ask for super user privileges while installing and updating packages.&#x20;
{% endhint %}

#### &#x20;<a href="#user-content-compileinstall" id="user-content-compileinstall"></a>
