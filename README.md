# YouTube sharing web application introduction
This is my assessment to implement a YouTube video sharing web application.
## 1. Project introduction:
- Backend database migration repo: https://github.com/ledaihoan/renect-youtube-sharing-database-migration
- Backend repo: https://github.com/ledaihoan/renec-youtube-sharing-backend
- Frontend (React) repo: https://github.com/ledaihoan/renect-youtube-sharing-ui
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
## 4. Database setup
- Clone repo at: https://github.com/ledaihoan/renect-youtube-sharing-database-migration
- Run yarn install
```shell
$ cd /path/to/renect-youtube-sharing-database-migration
$ yarn
```
- Run the command to init database
```shell
$ cd /path/to/renect-youtube-sharing-database-migration
# delete is optional param to remove existing instance in local machine
$ ./start_db.sh [delete]
```
- Run the command to make migration file: the script will create a timestamp for you. When make PR to git, please keep in mind that you should have the latest timestamp in compare to current git main branch state to make the migration work correctly
```shell
$ yarn migrate:make migration_script_name
```
- Migration run script: Change the env name with corresponding env.${DEPLOYMENT_ENV}.yaml
```shell
# For local, providing this variable is optional
export DEPLOYMENT_ENV=local
$ ./run_migration_with_env.sh $DEPLOYMENT_ENV
```
## 5. Running the application
- Start database migration as mentioned in Step4 first. Check connection with a SQL client like DBeaver or PgAdmin4, default connection string is postgresql://postgres:Renec2024@localhost:5432/postgres
- Start backend local: the URL will be http://localhost:8000 or http://renec-yt-sharing-backend:8000
```shell
$ cd /path/to/renec-youtube-sharing-backend
# To start service on local machine
$ ./run_service_with_docker.sh
# To stop service on local machine
$ ./stop_docker.sh
```
- Start ui local: the URL will be http://localhost:3000 or http://renect-yt-sharing-ui:3000
```shell
$ cd /path/to/renec-youtube-sharing-frontend
$ ./run_service_with_docker.sh
```
- Live version is at https://yt-sharing-app.technoma.tech (UI), https://yt-sharing-backend.technoma.tech (Backend)
## 6. Docker & deployment
- In assessment scope, I just provide local run as mentioned in Section 5.
- Linux base deployment script also provided in same folder (./run_service.sh $DEPLOYMENT_ENV)
- Please check carefully on prerequisites and environment install guide  in section 2 and 3 before process.
## 7. Usage
- Main screen: https://yt-sharing-app.technoma.tech
- Login Feature: Input your email (unique) with password, click Register for first time that you don't have registered account yet and Login to use existing account
- Share video button: available when you logged in
- Reaction buttons: available when you logged in
- Create video share post modal: just paste in a YouTube URL and the title & description should be parsed from the link automatically
## 8. Trouble shooting and common issues:
- NodeJS env permission: don't use root to process installation, use NVM for better env setup experience
- Docker: Check permission and post installation setup as mentioned in official link
- The installation and the running process should be done successfully as my guide and sample installation scripts are well-written to support compatibility for most popular OS

## MISC, improvements
As I have tight schedules while taking this assessment, I will try to update soon :)
- Unit test: Backend already setup, just add more unit test and change Jest code coverage setting threshold to enforce quality. I don't have unit test for frontend yet as I'm not so familiar with Story Book
- Web socket subscribe & notification: Yes, this also take about 1 more day
- Test plan:
  - API list
    - public: Register, Login, Search video posts
    - authenticated: Create Video posts, Up vote / down vote, search current users' reaction
  - API test:
    - payload validation
    - content behavior correctness
      - Register: should create account and check email unique
      - Password: should be 8 characters length minimum with at least 1 uppercase, 1 number and 1 special character
      - Login: should log in if given user and password is valid matched
      - Create video posts / reaction: should authenticate users and prevent unauthorized access (not logged in or wrong user resource ownership)
  - UI test:
    - test UI appearance specs
    - test UI content display correctness
    - test UI functional work properly as defined scenarios
    - test UI independently: with Mocked setup data / server
  - Non-functional test: 
    - test performance
    - test UI/UX metric
    - apply evaluation tools: Light House, .....
