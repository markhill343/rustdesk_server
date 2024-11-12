# Setting Up a New RustDesk Server (e.g., on Pi 5)

Follow these steps to set up a RustDesk server on your system.

## Step 1: Stop Existing Containers (Optional)

If you already have RustDesk containers running, stop and remove them with:

```bash
sudo docker stop hbbs hbbr
sudo docker rm hbbs hbbr
rm $HOME/hbbs/id_ed25519 $HOME/hbbs/id_ed25519.pub
```

## Step 2: Create Necessary Directories

Set up the required directories:

```bash
mkdir -p $HOME/hbbs
mkdir -p $HOME/hbbr
```

## Step 3: Pull the Latest Docker Image

Download the latest RustDesk server image:

```bash
docker pull rustdesk/rustdesk-server:latest
```

## Step 4: Run Containers with Restart Policies

Start the RustDesk server containers and set them to restart unless stopped:

```bash
sudo docker run -d --name hbbs \
  --restart unless-stopped \
  --network host \
  -v $HOME/hbbs:/root \
  rustdesk/rustdesk-server hbbs

sudo docker run -d --name hbbr \
  --restart unless-stopped \
  --network host \
  rustdesk/rustdesk-server hbbr
```

## Step 5: Verify Containers Are Running

Check the status of your containers:

```bash
sudo docker ps
```

## Step 6: Retrieve and Use the Public Key

Get the public key for client connections:

```bash
cat $HOME/hbbs/id_ed25519.pub
```

## Step 7: Connect the Client

To connect a client, enter the public IP in both the ID and Relay Server fields, and paste the retrieved public key into the RustDesk client.

## Step 8: Configure the Firewall (UFW)

Use Uncomplicated Firewall (UFW) to manage incoming and outgoing traffic for your server.

### Install UFW

```bash
sudo apt-get install ufw
```

### Allow Necessary Ports

Configure UFW to allow only the required ports for RustDesk and SSH.

```bash
# Deny all incoming by default and allow outgoing
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Allow SSH (replace 22 if using a different port)
sudo ufw allow 22/tcp

# Allow RustDesk Ports
sudo ufw allow 21115/tcp
sudo ufw allow 21116/udp
sudo ufw allow 21117/tcp
```

### Enable UFW

```bash
sudo ufw enable
```

### Check UFW Status

Verify the UFW settings:

```bash
sudo ufw status verbose
```

### Install Fail2Ban
```bash
sudo apt-get install fail2ban
```

Your RustDesk server should now be up and running, with the firewall configured to allow necessary traffic.
