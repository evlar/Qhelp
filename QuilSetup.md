
# Setting Up a Quilibrium Ceremony Client Node

To set up a Quilibrium Ceremony Client node and start acquiring $QUIL, follow these simplified steps. This guide includes session management with `tmux` and comprehensive process management instructions, now ordered to minimize the need for restarts.

## 1. Provision a VPS

Choose a VPS provider and set up a server running **Ubuntu 22.04**.

## 2. Access Your Server

Use SSH to connect to your VPS.

## 3. Install Prerequisite Software

Execute the following commands to install necessary software:

```bash
sudo apt -q update
sudo apt-get install jq tmux -y  # Includes tmux installation
sudo apt install git -y
git --version  # Ensure git version is 2.34.1 or later
```

## 4. Install Golang

Download and install Golang with these commands:

```bash
wget https://go.dev/dl/go1.20.14.linux-amd64.tar.gz
sudo tar -xvf go1.20.14.linux-amd64.tar.gz -C /usr/local
echo 'export GOROOT=/usr/local/go' >> ~/.bashrc
echo 'export GOPATH=$HOME/go' >> ~/.bashrc
echo 'export PATH=$GOPATH/bin:$GOROOT/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
go version  # Verify installation with go version go1.20.14 linux/amd64
```

## 5. Clone the Ceremony Client Repo

Clone the repository and navigate to the node directory:

```bash
git clone https://github.com/QuilibriumNetwork/ceremonyclient.git
cd ceremonyclient/node
```

## 7. Configure the firewall to allow necessary traffic:

  ```bash
  sudo ufw allow 22 && sudo ufw allow 8336 && sudo ufw allow 8337 && sudo ufw allow 8338 && sudo ufw allow 8317 && sudo ufw allow 8316 && sudo ufw enable
  sudo ufw status  # Confirm firewall settings
  ```

## 8. Start Node in a tmux session

Initiate a tmux session:

```bash
tmux new -s quilnode
```

Start the node within the tmux session:

```bash
GOEXPERIMENT=arenas go run ./...
```

Detach from the tmux session with `Ctrl+b` then `d` after confirming the node starts successfully.

## 6. Configure gRPC
- Edit `.config/config.yml`:

  ```bash
  nano ~/ceremonyclient/node/.config/config.yml
  ```
- Scroll to the bottom with the arrow keys to the line, `listenGrpcMultiaddr: ""`, and replace with:
  ```bash
  listenGrpcMultiaddr: /ip4/127.0.0.1/tcp/8337
  ```
- Save the file with `Ctrl+O`, and exit with `Ctrl+X`.

## Kill the Node Process

- Find the PID of your node process:

  ```bash
  ps -aux | grep go # PID for process ending in /exe/node
  ```

- Stop the node process:

  ```bash
  kill PID 
  ```

Replace `PID` with the actual process ID.

## 8. Restart Node

- Renitiate a tmux session:

  ```bash
  tmux new -s quilnode
  ```

- Start the node within the tmux session:

  ```bash
  GOEXPERIMENT=arenas go run ./...
  ```

- Detach from the tmux session with `Ctrl+b` then `d` after confirming the node starts successfully.

## Additional Options

- Get your Node Peer ID:

  ```bash
  cd ~/ceremonyclient/node
  GOEXPERIMENT=arenas go run ./... -peer-id
  ```
- Get your $QUIL balance:

  ```bash
  cd ~/ceremonyclient/node
  GOEXPERIMENT=arenas go run ./... -balance
  ```
