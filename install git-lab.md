# Install Gitlab as a container
> beware you need atleast 6GB of ram otherwise the app might crash or not being healthy.

## Install the Dependencies

The first thing we’ll do is install the required dependencies. Log in to your Ubuntu instance and install the required software with the command:
```
sudo apt install ca-certificates curl openssh-server apt-transport-https gnupg lsb-release -y
```
Next, we need to install the Community Edition of Docker. For this, we’ll add the official Docker GPG key with:
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
Next, add the Docker repository:
```
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
Update apt with the command:
```
sudo apt-get update
```
## install Docker Community Edition with:
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose -y
```
Add your user to the docker group with:
```
sudo usermod -aG docker $USER
```
Log out and log back in for the changes to take effect.

So far, so good. Let’s move on.

## Change the Default SSH Port
Because GitLab uses the default SSH port, you must change the default SSH server port. Otherwise, there’ll be a conflict. Open the SSH config file with:
```
sudo nano /etc/ssh/sshd_config
```
In that file, look for the line:

Port 22

Change that line to:
```	
Port 2022
```
```
sudo ufw allow 2022
```
## Create a New Docker Volume

We’re now ready to move on to the Docker side of things. The first thing we’ll do is create a new volume. First, create a directory to house the files with:
```
sudo mkdir -p /srv/gitlab
```
Next, create a directory that will house our Docker compose file with:
```
mkdir ~/docker-gitlab
```
Change into that directory with:
```
cd ~/docker-gitlab
```
Create a file to house environment variables with:
```
nano .env
```
Paste the following into that new file:
```
GITLAB_HOME=/srv/gitlab
```
Save and close the file.

## Create the Docker Compose File
Create a new compose file with:
```
nano docker-compose.yml
```
In that file, paste the following (make sure to change anything in bold to suit your environment/needs):
> you can add any gitlab.rb configs in this file as well.(see gitlab.rb ingithub web application)
```
version: '3.6' 			#Choose the version you want
services:
  web:
    image: 'gitlab/gitlab-ee:latest'
    restart: always
    hostname: 'gitlab.example.com'
    environment:
      GITLAB_OMNIBUS_CONFIG: |
        external_url 'https://gitlab.example.com'
        # Add any other gitlab.rb configuration here, each on its own line
    ports:
      - '80:80'
      - '443:443'
      - '22:22'
    volumes:
      - '$GITLAB_HOME/config:/etc/gitlab'
      - '$GITLAB_HOME/logs:/var/log/gitlab'
      - '$GITLAB_HOME/data:/var/opt/gitlab'
    shm_size: '256m'
```
Save and close the file.
----------------------------------------------------------------------------
## Deploy the Container
We’re now ready to deploy the container. To do that, issue the command:
```
docker-compose up -d
```
The deployment of the container will take some time (anywhere between 10-30 minutes, depending on the speed of your network connection), so either sit back and watch the output fly by or take care of some other task. When the deployment completes, you’ll need to access the automatically generated root password with the command:
```
sudo cat /srv/gitlab/config/initial_root_password
```
> You should see a long string of random characters that will serve as your root password login.

Accessing GitLab

Open a web browser and point it to http://SERVER (where SERVER is the IP address or domain of your server).

