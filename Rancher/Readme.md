Rancher
=======

This demo runs a Rancher server and one Rancher host with the agent running on it.

## Requirements
* VirtualBox
* Vagrant

## Usage
The Vagrant file contains definitions for two virtual machines, a Rancher server and a Rancher agent.

1. Create the Rancher server and agent
   Open a shell on your host and type
   ```
   vagrant up rancher-server
   ```
   followed by
   ```
   vagrant up rancher-agent-1
   ```

   A simple vagrant up should also work

2. Ensure the Rancher server is working.
   After the VM's have booted on host machine in browser navigate to http://localhost:9000

3. Ensure that your Rancher agent machine can connect to the Rancher server
   Login to the agent machine with
   ```
   vagrant ssh rancher-agent-1
   ```
   After logging in make a request to the Rancher server
   ```
   curl http://172.19.8.100:9000
   ```

4. Create the command for the agent to connect to the host
   Back to your browser on the host machine, in rancher, click on Infrastructure, then Add Host
   Select "Something Else" and enter use the IP address of your Rancher server http://172.19.8.100:9000
   Click next
   Set the agent host to 172.19.8.101 (step 4) and click on the clipboard icon (step 5 in the UI)
   Your clipboard should contain something similar to the text below
   sudo docker run -e CATTLE_AGENT_IP="172.19.8.101"  --rm --privileged -v /var/run/docker.sock:/var/run/docker.sock -v /var/lib/rancher:/var/lib/rancher rancher/agent:v1.2.2 http://172.19.8.100:9000/v1/scripts/9AB60D6BDAA0171D34DE:1483142400000:7Xf5xcrunyHoGNkhFaXYv6yFag

5. Add the agent as a host for the server to use
   Login back in (or open the SSH session) to the Rancher agen
   ```
   vagrant ssh rancher-agent-1
   ```
   Run the command generated in the previous steps

6. Test Rancher by creating an instance of Jenkins
    In the browser on the host go to Rancher server, Catalog, select Jenkins, select port number 8080

7. Test Jenkins
   When Jenkins has provisioned test it on your guest VM with curl
    ```
    sudo su -
    curl http://172.19.8.101:8080
    ```
    or
    ```
    sudo curl http://172.19.8.101:8080
    ```
    The response will be similar to the one below
    ```
    <html><head><meta http-equiv='refresh' content='1;url=/login?from=%2F'/><script>window.location.replace('/login?from=%2F');</script></head><body style='background-color:white; color:white;'>


    Authentication required
    <!--
    You are authenticated as: anonymous
    Groups that you are in:

    Permission you need to have (but didn't): hudson.model.Hudson.Administer
    -->
    ```
    NB: It may take some time to provision Jenkins depending on your download speed, the processing power of your host and the settings on the VM. My setup which is an i7 Quad Core laptop takes around 20 minutes to download and provision Jenkins.
    If you receive the following
    ```
    curl: (7) Failed to connect to <ip> port 8080: Connection refused
    ```    
    Then it could be because Jenkins and the sidekicks are still starting up, ipv6 is enabled (try curl with --ipv4), blocking firewall rules, it might be worth stopping and starting Jenkins from Rancher UI.
    If Jenkins still does not work then check the docker logs (you can do this with the Rancher UI) or run native Docker ps, Docker inspect and Docker log commands on the agent machine. The log file may for the Jenkins primary may end with something similar to
    ```
    --> setting agent port for jnlp
    --> setting agent port for jnlp... done
    Jul 07, 2017 11:22:45 AM hudson.model.UpdateSite updateData
    INFO: Obtained the latest update center data file for UpdateSource default
    Jul 07, 2017 11:22:46 AM hudson.model.DownloadService$Downloadable load
    INFO: Obtained the updated data file for hudson.tasks.Maven.MavenInstaller
    Jul 07, 2017 11:22:47 AM hudson.model.UpdateSite updateData
    INFO: Obtained the latest update center data file for UpdateSource default
    Jul 07, 2017 11:22:47 AM hudson.WebAppMain$3 run
    INFO: Jenkins is fully up and running
    Jul 07, 2017 11:24:59 AM hudson.model.DownloadService$Downloadable load
    INFO: Obtained the updated data file for hudson.tools.JDKInstaller
    Jul 07, 2017 11:24:59 AM hudson.model.AsyncPeriodicWork$1 run
    INFO: Finished Download metadata. 270,664 ms
    ```

8. Ensure the Jenkins container is running correctly
    On the host in the browser with Rancher select Infrastructure, then hosts, then view the Jenkins container stats

9. Access Jenkins via the browser
    On the host open another browser tab, enter  http://172.19.8.101:8080, this will load the Jenkins start page
 
Notes:
You can access the Rancher server UI with http://127.0.0.1:9000
Running Rancher server and agent on the SAME HOST has proved to be harder than expected hence two VM's.
Rancher server is working in the latest version of Docker (17.06)
Rancher agent needs a Docker version earlier than 17.04
* http://rancher.com/docs/rancher/v1.6/en/hosts/#supported-docker-versions
* https://github.com/rancher/rancher/issues/9192

Lots of community images available, Wordpress? Drupal? No problem. Simple to run an install, if using this VM and demo then you MAY need to create extra port mappings if you want to access the services on your host machine as localhost, for example, Jenkins and/or Wordpress can be made available in the host browser as http://localhost:8000 or http://localhost:80
