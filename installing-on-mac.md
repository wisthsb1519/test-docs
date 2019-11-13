# Installing on a Mac
-------------------

Verified on OSX v10.13.4 / High Sierra.

#### Retrieve Installation Package

Create a temporary folder to hold the install package and files.

    mkdir ~/inst
    cd ~/inst

Read how we use and store your data [here](https://github.com/algorand/go-algorand-doc/blob/master/downloads/installers/darwin_amd64/README.md). Copy the [installer](https://github.com/algorand/go-algorand-doc/blob/master/downloads/installers/darwin_amd64/install_master_darwin-amd64.tar.gz) from Github ([downloads/installers/darwin\_amd64](https://github.com/algorand/go-algorand-doc/blob/master/downloads/installers/darwin_amd64/)). Optionally, you can verify [SHA](https://github.com/algorand/go-algorand-doc/blob/master/downloads/installers/darwin_amd64/install_master_darwin-amd64.sha256) for the file.

Unzip the package (make sure to use the appropriate filename):

    tar -xf install_master_darwin-amd64.tar.gz

#### Installation Instructions

You are now ready to install! Run the installer from within your installation directory.

    ./update.sh -i -c stable -p ~/node -d ~/node/data -n

When the installer runs, it will pull down the latest update package from S3 for your platform and install it. The `-n `option above tells the installer to not auto-start the node. If the installation succeeds you'll be instructed to start the node manually. After configuring telemetry, run \`goal node start -d ~/node/data\` to start it.

#### Start Your Node

Follow the steps [here](/docs/introduction-installing-node#start-node) to enable telemetry and start your node!