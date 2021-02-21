# Install Neo node on Ubuntu

The instructions provided on neo.org require some gap-filling (at least for me). These are the instructions used to go from empty linux box to running neo-cli.

[//]: # "TODO: I should make these instructions into an ansible playbook."
## Step 0 - Install libraries

```
# https://docs.neo.org/docs/en-us/node/cli/setup.html
sudo apt-get install libleveldb-dev sqlite3 libsqlite3-dev libunwind8-dev 
```

## Step 1 - Install dotnet CLI
Download the x64 binary ASP.NET Core Runtime 5.0.x at https://dotnet.microsoft.com/download/dotnet/5.0. For this tutorial, I'm assuming you'll download the file to `$HOME/Downloads/aspnetcore-runtime-5.0.3-linux-x64.tar.gz` and will be putting source code in `$HOME/src`

```
# Create and enter dir for to-be extracted code
mkdir $HOME/src/dotnet && cd $_

# extract code to directory
tar -xf $HOME/Downloads/dotnet-sdk-5.0.103-linux-x64.tar.gz

# Add directory to PATH

```
# Make dotnet command available without specifying full path
echo 'export PATH="$PATH:$HOME/src/dotnet"' >> $HOME/.zshrc
# reload zsh
source $HOME/.zshrc
```

## Step 2 - Download and build neo-cli

```
# Clone the neo-node project and enter it
git clone https://github.com/neo-project/neo-node.git $HOME/src/neo-node
cd $HOME/src/neo-node/neo-cli
# Build it
dotnet restore
dotnet publish -c release -r linux-x64
```

## Step 3 - Download and install LevelDB

``` 
# Download
mkdir $HOME/Downloads/leveldb && cd $_
curl -L -O https://github.com/neo-project/neo-modules/releases/download/v3.0.0-preview5/LevelDBStore.zip
unzip LevelDBStore.zip
# Make available for neo-cli by adding to Plugins
mkdir $HOME/src/neo-node/neo-cli/bin/release/net5.0/linux-x64/Plugins && cd $_
cp $HOME/Downloads/leveldb/Plugins/LevelDBStore.dll .
```

## Step 5 - Run CLI

```
# Reference the built version of `neo-cli` when invoking command
echo 'alias neo-cli="~/src/neo-node/neo-cli/bin/release/net5.0/linux-x64/neo-cli"' >> ~/.zshrc
neo-cli
```