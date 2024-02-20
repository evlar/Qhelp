
# Setting Up a Quilibrium Ceremony Client Node

To set up a Quilibrium Ceremony Client node and start acquiring $QUIL, follow these simplified steps. This guide includes the use of tmux, a terminal multiplexer, to manage running processes and maintain access to the terminal for subsequent steps.

## 1. Provision a VPS

Choose a VPS provider and set up a server running **Ubuntu 22.04**.

## 2. Access Your Server

Use SSH to connect to your VPS.

## 3. Install Prerequisite Software

Execute the following commands to install necessary software:

```bash
sudo apt -q update
sudo apt-get install jq tmux -y  # Includes tmux installation
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

## 6. Use tmux to Manage Terminal Sessions

Before starting the node, initiate a tmux session to allow multitasking:

```bash
tmux new -s quilnode
```

## 7. Start the Node in Development Mode

In the tmux session, start the node in development mode. You can later detach from this tmux session and continue using the terminal:

```bash
GOEXPERIMENT=arenas go run ./...
```

To detach from the tmux session and return to your main terminal, press `Ctrl+b` then `d`.

## 8. Get Your Node Peer ID

To get your node peer ID, you do not need to restart the node. Simply reattach to the tmux session where the node is already running:

```bash
tmux attach-session -t quilnode
# Execute the command to get the peer ID, assuming it can be run without interrupting the node.
```

Remember to detach from the tmux session with `Ctrl+b` then `d` after running the command.

## 9. Enable gRPC

To participate in the network and earn $QUIL, enable gRPC:

- Edit `.config/config.yml` in your node directory.
- Set `listenGrpcMultiaddr` to `/ip4/127.0.0.1/tcp/8337`.

## 10. Configure Firewall

Allow necessary network traffic through the firewall:

```bash
sudo ufw allow 22 && sudo ufw allow 8336 && sudo ufw allow 8337 && sudo ufw enable
sudo ufw status  # Verify firewall configuration
```

## 11. Check Your $QUIL Balance

To check your $QUIL balance, reattach to the tmux session where your node is running:

```bash
tmux attach-session -t quilnode
GOEXPERIMENT=arenas go run ./... -balance
```

Detach from the tmux session when finished.