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
#### Basic server configuration
- Open a browser and navigate to http://localhost:8080
- Wait until the screen loads to the "Unlock Jenkins" screen
  - ![Image1](images/image1.png?raw=true "Image1")
- In your command prompt run `docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword` and copy the results to that page and hit continue
- On the next page, don't select "Install recommended plugins" or "Select which plugins to install".  Click the little 'x' in the top right corner
- You will be told Jenkins is ready and get the warning 
> "You've skipped creating an admin user. To log in, use the username: 'admin' and the administrator password you used to access the setup wizard."
- Click "Start using Jenkins"
- The first thing to do is set a password for the admin user incase you need to log back in.  From the top right select "admin" and then "Configure". Scroll to the very bottom of the page and replace the password with one of your choosing.  'netapp123' for example
- Click "Save"
- In the top left click on Jenkins, then select Manage Jenkins, and from the list choose Manage Plugins.  
- Choose the 'Available' tab from the top and wait for the page to load.  
- Once the page loads in the search box type 'docker '.  
- Notice the space after docker, thats important, but don't type the ''.  
- After you hit enter the list will filter.  
- You need to select "CloudBees Docker Build and Publish plugin" and "Docker plugin".  
- Then click at the bottom "Install without restart".  
- This step can take a few momements based on internet speed.  
  - If at some point you feel its hung or not progressing, from the left side menu select "Manage Plugins" and then "Update Center".  When all the items say "Success" procede.
#### Create new job
- Select Jenkins from the top left, then click "Create new jobs"
- Title the job 'webpage' and select pipeline for the type, then click 'OK'
- Under Build Triggers select Poll SCM.  Set the following schedule "M/5 \* \* \* \*".  That means check every five minutes, but not neccesary on the 5 minute mark.  
- Switch to the Pipeline section and the Definintion should be "Pipeline script from SCM".  This looks for a Jenkinsfile in the repositry we specifiy.
- For SCM choose Git and enter this reposity Url "http://git:3000/netapp/website.git"
- Scroll down and click "Save"
### 5. Run test
- On the left hand side select "Back to dashboard" and then "Open Blue Ocean"
- Select webpage and when prompted, click 'Run'
  - The process takes about 40 seconds give or take how long it takes to download the base container image the first time.
### 6. View results
- In your command prompt run `docker run -d --rm -it --name ci_cd -p 80:80 myweb` to start the container image we just built.
- Open a browser and navagate to http://localhost
- You should see this.
- ![Image1](images/image1.png?raw=true "Image1")
