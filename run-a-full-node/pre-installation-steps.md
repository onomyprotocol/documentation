# Installation Steps

Binaries are provided on the GitHub releases page. Scripts are provided to create binaries.

Follow these steps to obtain the binaries:

1. Download the bin.sh scripts \
   \
   `wget https://raw.githubusercontent.com/onomyprotocol/onomy/dev/deploy/testnet/scripts/bin.sh`
2. dd the executable permission using `chmod +x bin.sh`
3. run the script using `bash bin.sh`

This script downloads and compiles needed binaries. A new directory `.onomy` will be created in your home directory. All the source code will be downloaded in `.onomy/src` directory and compiled binaries will be in `.onomy/bin` directory.

{% hint style="info" %}
NOTE: Do not run the script as a super user. If you run script as a super user, then the .onomy directory will be created in home directory of super user. Script will ask for super user privileges while installing and updating packages.&#x20;
{% endhint %}

#### &#x20;<a href="#user-content-compileinstall" id="user-content-compileinstall"></a>
