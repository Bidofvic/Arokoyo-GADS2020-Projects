# Course: Google Cloud Fundamentals: Getting Started with Deployment Manager and Cloud Monitoring

## Objectives:


In this lab, you will learn how to:

 - Create a Deployment Manager deployment.

 - Update a Deployment Manager deployment.

 - View the load on a VM instance using Cloud Monitoring.

# Task1: Create a Deployment Manager deployment.

##Steps

1. Open the Cloud Shell prompt

2.  Use the gcloud command to grab your assigned Zone:

	export My_Zone=us-central1-a

3. Download editable Deployment Manager template:

	gsutil cp gs://cloud-training/gcpfcoreinfra/mydeploy.yaml mydeploy.yaml

4. Use the sed command to replace the PROJECT_ID placeholder string with your Google Cloud Platform project ID using this command:

	sed -i -e "s/PROJECT_ID/$DEVSHELL_PROJECT_ID/" mydeploy.yaml


5. Also, use the sed command to replace the ZONE placeholder string with your assigned Google Cloud Platform zone using this command:

	sed -i -e "s/ZONE/$MY_ZONE/" mydeploy.yaml

6. View the mydeploy.yaml file to check your modifications using this command:

	cat mydeploy.yaml

Note: Ensure the zone that is named on the zone: and machineType: lines in your file matches the zone to which Qwiklabs assigned you. Also ensure that the GCP project ID on the network: line in your file matches the project ID to which Qwiklabs assigned you.

7. Let's build a deployment from the template using the command:

	gcloud deployment-manager deployments create my-first-depl --config mydeploy.yaml

Note: When the deployment operation is complete, the gcloud command displays a list of the resources named in the template and their current state.

8. Confirm that the deployment was successful by navigating to three burger menu, click Compute Engine > VM instances
There you will see that a VM instance called my-vm has been created, as specified by the template.

9. Scroll down to the Custom metadata section and confirm that the startup script you specified in your Deployment Manager template has been installed.


#Task2: Update a Deployment Manager deployment

##Steps

1. Return to your Cloud Shell prompt. Launch the nano text editor to edit the mydeploy.yaml file with the command:

	nano mydeploy.yaml

2. Find a line that sets the startup script value, value: "apt-get update", and edit it in a way that looks like this:

      value: "apt-get update; apt-get install nginx-light -y"

Note: The YAML templating language relies on indented lines as part of its syntax, so do not alter the space at the beginning of the line. As you edit the file, ensure that the v in the word value in this new line is immediately below the k in the word key on the line above it.

3. To save your edited file, press Ctrl+O and then press Enter

4. To exit the nano text editor, Press Ctrl+X 

5. Return to your Cloud Shell prompt. Initiate the Deployment Manager to update your deployment to install the new startup script using this command line:

	gcloud deployment-manager deployments update my-first-depl --config mydeploy.yaml

Note: Wait for the gcloud command to display a message confirming that the update operation was completed successfully.

6. In the GCP console, on the Navigation menu, click Compute Engine > VM instances.

7. Click on the my-vm instance's name to open its VM instance details pane.

8. Scroll down to the Custom metadata section. Confirm that the startup script has been updated to the value you declared in your Deployment Manager template.


# Task 3: View the Load on a VM using Cloud Monitoring

##Step

1. On the Navigation menu of the GCP Console, click Compute Engine > VM instances.

2. Use the ssh command to open a command prompt on the my-vm instance:
In the Cloud Shell prompt, connect to my-vm using:

	gcloud compute ssh my-vm

3. In the ssh session on my-vm, execute this command to create a CPU load:

	dd if=/dev/urandom | gzip -9 >> /dev/null &

Note: This Linux pipeline forces the CPU to work on compressing (gzip) a continuous stream of random data. Ensure you leave your SSH session window open while you proceed with the lab.

4. Setup a Monitoring workspace that's tied to your GCP project by clicking on the Navigation menu > Monitoring

5. Wait for your workspace to be provisioned. Once the Monitoring dashboard opens, your workspace is ready.

6. Click on Settings option on the left panel and confirm that the GCP project which Qwiklabs created for you is shown under the GCP Projects section.

7.Under the Settings tab menu, click Agent. Using your VM's open SSH window and the code shown on the Agents page, install both the Monitoring and Logging agents on your project's VM.

8. Once both of the agents have been installed on your project's VM, click Metrics Explorer under the main Cloud Monitoring menu on the far left.

9. In the Metric pane of Metrics Explorer, select the resource type GCE VM instance and the metric CPU usage.

In the resulting graph, you will notice that CPU usage increased sharply a few minutes ago.

10. Terminate your workload generator and return to your ssh session on my-vm and enter this command:

	kill %1
