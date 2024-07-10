# YouTube sharing web application introduction
This is my assessment to implement a YouTube video sharing web application.
## 1. Project introduction:
- Backend database migration repo: https://github.com/ledaihoan/renec-youtube-sharing-database-migration
- Backend repo: https://github.com/ledaihoan/renec-youtube-sharing-backend
- Frontend (React) repo: https://github.com/ledaihoan/renec-youtube-sharing-ui
- Live URL: https://yt-sharing-app.technoma.tech
### 1.1 Initial constraints & assumptions
- Customer / users: The users who want to share YouTube video with others in same organization or network.
- Product owner: Remitano team.
- Development team size: 1.
- Team member technical competencies: NodeJS, ReactJS, Linux & Docker.
### 1.2 Project core features
- YouTube videos sharing: 
  - Allow users to create or remove shared video entries.
  - Allow users to view video entries shared by others.
- User authentication: An user need to register and login using an account to use the app main feature.
  - Create new or remove share video entries owned by.
  - Up/Down vote for the share entries in the app.
  - Unauthenticated users still can see video lists but can not add reaction to the entries or to create/remove owned entries.
- WebSocket notification:
  - Notification about entries update: Authenticated users can receive realtime notification about new sharing entries published to the app by others.
## 2. Prerequisites
- Suggested local machine environment: Use Linux/UNIX-based operating systems like Ubuntu, Fedora or Mac. The current project can support Windows but not well-tested.
- Software dependencies:
  - Shell tools: curl, wget, vim
  - Git (latest stable version): To pull / update source repositories.
  - NVM (latest LTS): For easier installing and managing NodeJS environment.
  - NodeJS with yarn (latest LTS): Run time of the projects.
  - Docker (latest LTS): quick setup & better compatible environment for across various device configurations.
## 3. Installation & Configuration guide
- Don't run the script as root. You should create admin user on linux with sudo permissions
### For who want to quickly step on next part:
- Clone or download zip file from git repository at: https://github.com/ledaihoan/common-dev.git
- Run the installation scripts are at 0-setup-base folder and currently support MacOS, Ubuntu and Fedora. You can also find that the scripts might also compatible with other Linux distros with little edit.
```shell
$ cd /path/to/common-dev/0-setup-base
$ ./install_env.sh
```
The script may still contain minor issues, so I suggest to keep looking on the step instructions below
### 3.1 Install NVM
- Official instruction link: https://github.com/nvm-sh/nvm

- Run installation script
```shell
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
# See the instruction in the terminal and check if you need to do some post-installation scripts.
# In most cases, just close the current terminal and open new one then you can see that nvm is available to use
```
### 3.2 Install NodeJS
- At the time of writing, the latest Node LTS version is 20.15.0. It's suggested that you should install the same main lts version 20.x.y
```shell
$ nvm install 20.15.0 # can use nvm install --lts
$ npm install -g yarn # yarn suggested, you may also install some cli like @nestjs/cli
```
### 3.3 Install Docker
- Official instruction: https://docs.docker.com/engine/install/
- Ubuntu script (other Linux-based almost the same, also provided in the official instruction):
```shell
#!/usr/bin/env bash
# Add Docker's official GPG key:
$ sudo apt-get update
$ sudo apt-get install -y ca-certificates curl
$ sudo install -m 0755 -d /etc/apt/keyrings
$ sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
$ sudo chmod a+r /etc/apt/keyrings/docker.asc
# Add the repository to Apt sources:
$ echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
$ sudo apt-get update
# Install docker
$ sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
- Docker desktop is recommended for MacOS and Windows: https://www.docker.com/products/docker-desktop/
- Post installation script for Linux-based OS:
```shell
$ sudo groupadd -f docker
$ sudo usermod -aG docker $USER
# Remember to enter this to make the change to take effect immediately in current shell. Or you need to logout/restart to make the changes take effect
$ newgrp docker
# Test docker run with out sudo
$ docker run hello-world
```
