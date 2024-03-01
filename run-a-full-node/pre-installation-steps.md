# Installation Steps

Obtaining the binary can be done by either of the following methods:

* [Download Binary from GitHub](pre-installation-steps.md#download-binary-from-github)
* [Compile Binary Locally Using Script](pre-installation-steps.md#compile-binary-locally)
* [Compile Binary Manually](pre-installation-steps.md#compile-binary-manually)

### Download Binary from GitHub

1. First make sure you have golang installed and $PATH is pointing to where go binary is installed. Also set the $GOPATH environment variable.
2. Head over to the [Releases Page](https://github.com/onomyprotocol/onomy/releases) and download the latest version of the Onomy Binary.
   * Default structure for Onomy files:

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



### Compile Binary Locally Using Script

1. Download the bin.sh script\
   \
   `wget https://raw.githubusercontent.com/onomyprotocol/onomy/main/deploy/scripts/bin-mainnet.sh`
2. Add the executable permission using `chmod +x bin-mainnet.sh`
3. Run the script using `bash bin-mainnet.sh`
4. Restart terminal so changes made in environment can take effect

This script downloads and compiles Onomy binary. A new directory `.onomy` will be created in your home directory. All the source code will be downloaded in the `.onomy/src` directory. Compiled binary will be in `.onomy/bin` directory.



## Compile Binary Manually

1. Create onomy Directory Tree&#x20;

We can start a full node with Cosmovisor which changes and uses new binary automatically after a chain upgrade.&#x20;

`mkdir –p .onomy/bin`

Create Cosmovisor directory if using Cosmovisor&#x20;

`mkdir –p .onomy/cosmovisor`

2. Install Necessary Packages&#x20;

Install packages needed to build Onomy binary.

This example is for ubuntu based systems

```
sudo apt-get update 
sudo apt-get upgrade -y 
sudo apt-get install curl nano ca-certificates tar git jq perl python3 moreutils wget nodejs make hostname crudini cmake -y
```

3. Install GO&#x20;

Download the latest version of Go and extract it.&#x20;

```
curl -LO https://go.dev/dl/go1.20.1.linux-amd64.tar.gz 
tar -C $HOME -xzf go1.20.1.linux-amd64.tar.gz
```

Setup environment variables&#x20;

```
export PATH=$PATH:$HOME/go/bin
export GOPATH=$HOME/go
```

Check if go installation is working&#x20;

`go version`&#x20;

4. Compile Onomy

Download onomy repo and compile binary. Be sure to use the latest release.

```
git clone -b v1.1.4 https://github.com/onomyprotocol/onomy.git 
cd onomy 
make build 
```

If using Cosmovisor, move the binary to Cosmovisor directory &#x20;

`mv onomyd $HOME/.onomy/cosmovisor/genesis/bin/onomyd`

Otherwise, we move it to bin directory&#x20;

`mv onomyd $HOME/.onomy/bin/onomyd`

&#x20;

5. Compile Cosmovisor&#x20;

&#x20;Download and compile Cosmovisor&#x20;

```
git clone -b cosmovisor-v1.0.1 https://github.com/onomyprotocol/onomy-sdk 
cd onomy-sdk 
make cosmovisor 
cp cosmovisor/cosmovisor $HOME/.onomy/bin/cosmovisor
```

6. Post-installation&#x20;

&#x20;Make binaries executable&#x20;

```
chmod +x $ONOMY_HOME/bin/* 
chmod +x $ONOMY_HOME/cosmovisor/genesis/bin/* 
```

&#x20;Add onomy paths to PATH variable&#x20;

```
export PATH=$PATH:$HOME/.onomy/bin 
export PATH=$PATH:$HOME/.onomy/cosmovisor/genesis/bin
```

&#x20;Add to bashrc to make changes persistent&#x20;

```
echo "export GOPATH=$HOME/go" >> ~/.bashrc  
echo "export PATH=$PATH" >> ~/.bashrc
```

&#x20;These variables are needed only for Cosmovisor&#x20;

```
echo "export DAEMON_HOME=$ONOMY_HOME/" >> ~/.bashrc 
echo "export DAEMON_NAME=onomyd" >> ~/.bashrc 
echo "export DAEMON_RESTART_AFTER_UPGRADE=true" >> ~/.bashrc 
```



{% hint style="danger" %}
NOTE: Do not run the script as a super user. If you run script as a super user, then the .onomy directory will be created in home directory of super user. Script will ask for super user privileges while installing and updating packages.&#x20;
{% endhint %}

