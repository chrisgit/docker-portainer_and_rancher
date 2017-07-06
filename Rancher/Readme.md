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
   Set the agent host to 172.19.8.101 (step 4)and click on the clipboard icon (step 5 in the UI)
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
   When Jenkins has provisioned test it inside the Rancher agent machine
    ```
    sudo su -
    curl http://localhost:8080
    ```
    NB: It may take some time to provision Jenkins depending on your download speed, the processing power of your host and the settings on the VM
    If you receive the following
    ```
    curl: (7) Failed to connect to localhost port 8080: Connection refused
    ```
    Then it could be because Jenkins and the sidekicks are still starting up, ipv6 is enabled (try curl with --ipv4), blocking firewall rules, it might be worth stopping and starting Jenkins from Rancher UI.
    If Jenkins still does not work then check the docker logs (you can do this with the Rancher UI) or run native Docker ps and Docker inspect commands on the agent machine.

8. Ensure the Jenkins container is running correctly
    On the host in the browser with Rancher select Infrastructure, then hosts, then view the Jenkins container stats

9. Test Jenkins
    On the host open another browser tab, enter http://localhost:8080, this will load the Jenkins start page
 
Notes:
You can access the Rancher server UI with http://127.0.0.1:9000
Running Rancher server and agent on the SAME HOST has proved to be harder than expected hence two VM's.
Rancher server is working in the latest version of Docker (17.06)
Rancher agent needs a Docker version earlier than 17.04
* http://rancher.com/docs/rancher/v1.6/en/hosts/#supported-docker-versions
* https://github.com/rancher/rancher/issues/9192
