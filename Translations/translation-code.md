# Course: Architecting with Google Kubernetes Engine - Foundation

## Objectives:


In this lab, you will learn how to perform the following tasks:

 - Create a Compute Engine virtual machine using the Google Cloud Platform (GCP) Console.

 - Create a Compute Engine virtual machine using the gcloud command-line interface.

 - Connect between the two virtual machine (VM) instances.

# Task1: Create a Compute Engine virtual machine instance using the Google Cloud Platfform Console

Step1:  Create a Compute Engine VM instance 

	gcloud compute instances create "my-vm-1" --machine-type "n1-standard-1"  --image-project "debian-cloud" --image "debian-9-stretch-v20190213" --subnet "default" --tags http

	gcloud compute firewall-rules create allow-http --action=ALLOW --destination=INGRESS --rules=http:80 --target-tags=http

Step2: Choose zone close to your geographical region using this partial command:

	gcloud compute zones list | grep

Step3: Set your default zone using the gcloud command line below:

	gcloud config set compute/zone us-central1-b

Step4: Create another Compute Engine virtual machine instance using the gcloud command: 

	gcloud compute instances create "my-vm-1" --machine-type "n1-standard-1"  --image-project "debian-cloud" --image "debian-9-stretch-v20190213" --subnet "default"

#Task2: Connect between the two VM instances

Step1: Connect to my-vm-2 using:

	gcloud compute ssh my-vm-2

from  my-vm-2 ping my-vm-1:

	ping -c 4 my-vm-1

Step2: Use the ssh command to open a command prompt on my-vm-1 from my-vm-2

	ssh my-vm-1

Step3: At the command prompt on my-vm-1, install the Nginx web server:

	sudo apt-get install nginx-light -y

Step4: Use the nano text editor to add a custom message to the home page of the web server:

	sudo nano /var/www/html/index.nginx-debian.html

Step5: Use the arrow keys to move the cursor to the line just below the h1 header. Add text like this, and replace YOUR_NAME with your name:

	Hi from Victor Arokoyo

Step6: At the command prompt on my-vm-1, confirm that the web server is serving your new page by executing the below command:

	curl http://localhost/

Response: The response will be the HTML source of the web server's home page, including your line of custom text.

Step 7:  Exit the command prompt on my-vm-1, using the 'exit' command:

	exit

Step8: Return to the command prompt on my-vm-2, confirm that my-vm-2 can reach the web server on my-vm-1 by executing the below command:

	curl http://my-vm-1/

Response: The response will again be the HTML source of the web server's home page, including your line of custom text.


Step9:  Get the external IP of the my-vm-1 instance using this command:

	gcloud compute instances list --zone us-central1-a

Paste the copied IP address of my-vm-1 into a new browser tab and hit enter. You will see your web server's home page, including your custom text.

	

