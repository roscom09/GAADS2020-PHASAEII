# Google Cloud Fundamentals: Getting Started with Compute Engine
## Objectives:
In this lab, you will learn how to perform the following tasks:
  - Create a Compute Engine virtual machine using the Google Cloud Platform (GCP) Console.
  - Create a Compute Engine virtual machine using the gcloud command-line interface.
  - Connect between the two instances.
## *Steps*

1. Create a virtual machine using the GCP Console
 - In GCP console, on the top right toolbar, click the Open Cloud Shell button and click continue.
 - To create a virtual machine named "my-vm-1" type

```
gcloud compute instances create "my-vm-1" \ 
--machine-type "n1-standard-1" \
--image-project "debian-cloud" \
--image "debian-9-stretch-v20190213" \
--subnet "default" --tags http
```
```
gcloud compute firewall-rules create "allow-http" --action=ALLOW --destination=INGRESS --rules=http:80 --target-tags=http
```
2. Create a virtual machine using the gcloud command line 
  - To display a list of all the zones in the region to which Qwiklabs assigned you, enter this partial command gcloud compute zones list | grep followed by the region that Qwiklabs or your instructor assigned you to.
For example, to display all the zones in us-central1 region.
```
gcloud compute zones list | grep us-central1
``` 
 - Choose a zone from that list other than the zone to which Qwiklabs assigned you. For example, if Qwiklabs assigned you to region us-central1 and zone us-central1-a you might choose zone us-central1-b.
```
gcloud config set compute/zone us-central1-b
```
 - To create a VM instance called my-vm-2 in that zone, type: 

```
gcloud compute instances create "my-vm-2" \
--machine-type "n1-standard-1" \
--image-project "debian-cloud" \
--image "debian-9-stretch-v20190213" \
--subnet "default"
```

3. Connect between VM instances
 - Use the ping command to confirm that my-vm-2 can reach my-vm-1 over the network:
 - connect to my-vm-2 
```
gcloud compute ssh my-vm-2
```
 - ping my-vm-1 from my-vm-2 
```
ping -c 4 my-vm-1
```
  *This ping operation will automatically abort after the fourth attempt*

 - Use the ssh command to open a command prompt on my-vm-1:
```
ssh my-vm-1
```
 -At the command prompt on my-vm-1, install the Nginx web server:
```
sudo apt-get install nginx-light -y
```
 - Use the nano text editor to add a custom message to the home page of the web server:
```
sudo nano /var/www/html/index.nginx-debian.html
```
 -Use the arrow keys to move the cursor to the line just below the h1 header. Add text like this, and replace YOUR_NAME with your name:
```
Hi from RASAQ
```
 -Exit the editor and confirm that the web server is serving your new page. At the command prompt on my-vm-1, execute this command:
```
curl http://localhost/
```
  *The response will be the HTML source of the web server's home page, including your line of custom text.*
 - To exit the command prompt on my-vm-1, execute this command:
```
exit
```
 *You will return to the command prompt on my-vm-2.*
 - To confirm that my-vm-2 can reach the web server on my-vm-1, at the command prompt on my-vm-2, execute this command:
```
curl http://my-vm-1/
```
 *The response will again be the HTML source of the web server's home page, including your line of custom text.*
 - To get the external IP of the my-vm-1 instance
```
gcloud compute instances list --zone us-central1-b
```
 -Copy the my-vm-1 external IP, paste into a new browser and press enter.
  *You will see your web server's home page, including your custom text.* 

### This is the end of the lab
