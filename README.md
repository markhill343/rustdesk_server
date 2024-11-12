# Creating New Rustdesk Server (for example on pi5)

## Step 1: Stop Existing Containers (optional)
sudo docker stop hbbs hbbr
sudo docker rm hbbs hbbr
rm $HOME/hbbs/id_ed25519 $HOME/hbbs/id_ed25519.pub

## Step 2: Create dirs
mkdir -p $HOME/hbbs
mkdir -p $HOME/hbbr

## Step 3: Pull latest Image
docker pull rustdesk/rustdesk-server:latest

## Step 4: Run Containers with Restart Policies
sudo docker run -d --name hbbs \
  --restart unless-stopped \
  --network host \
  -v $HOME/hbbs:/root \
  rustdesk/rustdesk-server hbbs

sudo docker run -d --name hbbr \
  --restart unless-stopped \
  --network host \
  rustdesk/rustdesk-server hbbr

# Step 5: Verify Containers are Running
sudo docker ps

# Step 6: Retrieve and Use the Public Key
cat $HOME/hbbs/id_ed25519.pub

# Step 7: Connect the Client
Write the public IP in ID server and relay server and the yey into the rust desk client.