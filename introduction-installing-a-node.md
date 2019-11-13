# Introduction - Installing a Node
--------------------------------

The current release includes a single installation with a simplified auto-update mechanism. A node installation consists of two folders: the _binaries_ (bin) and the _data_ (data) folders. The bin folder can be anywhere you choose, but we recommend the location  `~/node`, if not using RPM or DEB installs. We will reference this location later in the documentation so remember to replace with the name that you chose if different. We assume this folder is dedicated to Algorand binaries and will archive it before each update. Note that we do not currently delete anything, but instead we will overwrite our own binaries and add new ones.

When installing for the first time we will ask you to specify a data folder. We recommend using a location under your node folder, e.g. `~/node/data`. See [Node Overview](/docs/node-overview) for a detailed list of all files that are installed on your computer and their functions. You can also set an environment variable that points to the data directory and goal will use this location if a specific data folder is not specified.

`export ALGORAND_DATA=~/node/data`

When installing with DEB or RPM packages the binaries will be installed in the  `/usr/bin` and the data directory will be set to `/var/lib/algorand`. It is advisable in these installs that you add the following export to your shell config files.

`export ALGORAND_DATA=/var/lib/algorand`

Also when installing with the DEB or RPM packages, kmd related files such as the kmd token file will be written into the `${HOME}/.algorand/kmd-version` directory. 

#### Installation Overview

Installing a new node is generally a 3 to 4-step process and will depend on the operating system. Choose your operating system below for a step-by-step guide to setting up your environment and installing the correct package for your platform.

1.  [Installing on a Mac](installing-mac)
2.  [Installing on Ubuntu](installing-ubuntu)
3.  [Installing in Docker](installing-docker)
4.  [Installing on Other Linux Distros](installing-other-linux-distros)

#### Configure Telemetry

Algod is instrumented to provide telemetry so we have insight into the software's performance and usage.  Telemetry is disabled by default and as so no data will be shared with Algorand Inc.  If you wish to enable telemetry, you can also provide a readable name for your node to make it easier to identify when we review telemetry.  Follow the commands below to enable and configure telemetry, replacing <name> with your desired hostname (e.g. 'SarahsLaptop').

    cd ~/node

Or if you wish to just enable telemetry without providing a host name:

    ./diagcfg telemetry enable
    

To enable or disable telemetry at any time, you can do so by running the following command:

    ./diagcfg telemetry enable|disable
    

If and of these commands report an error instead of your _node_ and _GUID_ values, double-check that you have completed all of the steps outlined for installing on your particular platform.

Running the diagcfg commands will create and update the logging configuration settings stored in ~/.algorand/logging.config.

#### Start Node

This step should not be needed if you followed one of the Linux install guides. 

Now you are ready to start your node! We will assume that you are in your node directory and that your data directory is named `data`. Run the following command:

    ./goal node start -d data

You should now have a running Algorand node! You can verify the daemon is running with:

    pgrep algod

If it outputs a number (i.e., a process ID) then algod is running.

#### Sync Node with Network

When you first start a node, it will need to sync with the network. This process can take a while as your node is loading up the current ledger and catching up to the rest of the network. You can check the status of your node by running the following goal command:

    ./goal node status -d data

Or watch the process live with our carpenter tool. It is a long-running program so Ctrl+c to stop watching.

    ./carpenter -d data

The `goal node status` command will return information about the node and what block number it is currently processing. When your node is caught up with the rest of the network, the "Sync Time" will be 0.0 as in the example response below (if on MainNet, some details will be different).

    Last committed block: 125064
    Time since last block: 3.1s
    Sync Time: 0.0s
    Last consensus protocol: https://github.com/algorandfoundation/specs/tree/5615adc36bad610c7f165fa2967f4ecfa75125f0
    Next consensus protocol: https://github.com/algorandfoundation/specs/tree/5615adc36bad610c7f165fa2967f4ecfa75125f0
    Round for next consensus protocol: 125065
    Next consensus protocol supported: true
    Genesis ID: testnet-v1.0
    Genesis hash: SGO1GKSzyE7IEPItTxCByw9x8FmnrCDexi9/cOUJOiI=

#### Check for Updates

If you installed with the RPM or DEB packages, the updating mechanism is covered on the respective install page. For other installs, you can check for and install the latest update, by running `./update.sh -d ~/node/data` at any time from within your node directory. It will query S3 for available builds and see if any are newer builds than your installed version. To force an update, you can run `./update.sh -i -c stable -d ~/node/data` again. If there is a newer version, it’s downloaded and unpacked before we shut down your node, back up your files, and install the update. If any part of the process fails, we attempt to restore your previous version (bin and data) and restart the node. If it succeeds, we’ll start the new version of the node even if it wasn’t running when you initiated the update.  You can include the `-n` option to skip restarting algod after installing / updating.