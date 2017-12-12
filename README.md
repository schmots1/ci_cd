# ci_cd
## This is a self-contained CI/CD demo/lab that can run on a laptop.  The resource requirements are very low, less than 2 gigs of RAM.
The steps to run this lab are as follows;
- Install Docker
- Clone or download the CI_CD repository
- Start the three docker containers
- Configure Jenkins
- Run test
- View result
- Make a change
- Re-run test
- View result
### 1. Install Docker
#### Download Docker for your system
 - Windows - https://download.docker.com/win/stable/Docker%20for%20Windows%20Installer.exe
 - Mac - https://download.docker.com/mac/stable/Docker.dmg
 - Linux - Run the following commands
 ```
 curl -fsSL get.docker.com -o get-docker.sh 
 sudo sh get-docker.sh
 ```
 #### Follow the steps in the installer.
 For Linux be sure to add your normal user to the docker group `sudo usermod -a -G docker <user>`
 ### 2. Clone or download the CI_CD repository.
 For cloning;
 `git clone https://github.com/schmots1/ci_cd.git`
 
 For downloading;
 Download https://github.com/schmots1/ci_cd/archive/master.zip and unzip it.
 
 From a command prompt navigate to the directory that you cloned or unzipd.
 ### 3. Start the three docker containers
 In the ci_cd (or ci_cd-master if you downloaded the zip) directory there is a file called ci_cd.txt.  Linux and Mac users can start all three containers by running `sh ci_cd.txt`.  Windows users will need to copy the text from that file into the command prompt.
 
 You know they are started correctly when the output of `docker ps` shows something like this;
 ```
 $ docker ps
CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS                                           NAMES
0b4cf0d6e9ea        jenkinsci/blueocean   "/bin/tini -- /usr..."   3 seconds ago       Up 1 second         0.0.0.0:8080->8080/tcp, 50000/tcp               jenkins
294b0a71ec83        registry:2            "/entrypoint.sh /e..."   4 seconds ago       Up 3 seconds        0.0.0.0:5000->5000/tcp                          registry
710ffa1eaf65        gogs/gogs             "/app/gogs/docker/..."   4 seconds ago       Up 3 seconds        0.0.0.0:3000->3000/tcp, 0.0.0.0:10022->22/tcp   git
```
This just started a Jenkins (www.jenkins.io) system, a Gogs git respository (www.gogs.io) and a local docker registry (www.docker.com).  These systems run on your local host and are available via the localhost address on the following ports.
- Jenkins:8080
- Gogs:3000
- Registry:5000
### 4. Configure Jenkins
- Open a browser and navigate to http://localhost:8080
- Wait until the screen loads to the "Unlock Jenkins" screen
  - ![Image1](images/image1.png?raw=true "Image1")
