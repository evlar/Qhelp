To set up a Quilibrium Ceremony Client node to acquire $QUIL, follow these simplified steps:

1. Provision a VPS:

Choose a VPS provider and set up a server running Ubuntu 22.04.

2. Access Your Server:

Use SSH to connect to your VPS.

3. Install Prerequisite Software:
```bash
   sudo apt -q update
   sudo apt-get install jq -y
   git --version  # Ensure git version is 2.34.1
   ```

4. Install Golang:

```bash
   wget https://go.dev/dl/go1.20.14.linux-amd64.tar.gz
   sudo tar -xvf go1.20.14.linux-amd64.tar.gz -C /usr/local
   echo 'export GOROOT=/usr/local/go' >> ~/.bashrc
   echo 'export GOPATH=$HOME/go' >> ~/.bashrc
   echo 'export PATH=$GOPATH/bin:$GOROOT/bin:$PATH' >> ~/.bashrc
   source ~/.bashrc
   go version  # Check for go version go1.20.14 linux/amd64
   ```

5. Clone the Ceremony Client Repo:

```bash
   git clone https://github.com/QuilibriumNetwork/ceremonyclient.git
   cd ceremonyclient/node
   ```

6. Start the Node in Development Mode (for initial setup and testing):

```bash
   GOEXPERIMENT=arenas go run ./...
   ```

7. Get Your Node Peer ID (for networking with other nodes):

```bash
   GOEXPERIMENT=arenas go run ./... -peer-id
   ```
8. Enable gRPC (to participate in the network and earn $QUIL):

    - Edit .config/config.yml in your node directory.

    - Set listenGrpcMultiaddr to /ip4/127.0.0.1/tcp/8337.

9. Configure Firewall (to allow necessary network traffic):

```bash
   sudo ufw allow 22 && sudo ufw allow 8336 && sudo ufw allow 8337 && sudo ufw enable
   sudo ufw status  # Confirm firewall status
   ```

10. Restart Your Node (to apply changes and start earning $QUIL):

```GOEXPERIMENT=arenas go run ./...```

11. Check Your $QUIL Balance (after letting your node run for some time):

```bash
    GOEXPERIMENT=arenas go run ./... -balance
    ```

