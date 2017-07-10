##Terraform Standards

###Remote State Configuration

Remote state configuration will use AWS S3 bucket to provide collaboration with the cloud
operations team.  We will use separate folders for each state file to provide isolation
between environments, applications and global services.

>terraform remote config \
    -backend=s3 \
    -backend-config="bucket=(YOUR_BUCKET_NAME)" \
    -backend-config="key=terraform.tfstate" \
    -backend-config="region=(YOUR_BUCKET_REGION)" \
    -backend-config="encrypt=true"


###Terraform Project Layout

To provide for proper isolation and minimizing the impact of changes, multiple project files
will be used.  The recommendation for project files will be to have isolated files for
each VPC environment (**_Prod_**, **_QA_**, **_Dev_**, **_MGMT_**). Additional projects
can be used to isolate application environments (**_Web Apps_**, **_Back-End Apps_**).


* Global
	IAM
	Route 53

* Prod
	* VPC
	* Services
		* Web-Apps
		* Backend-apps
	* Data Storage 
	
* QA
	* VPC
	* Services
		* Web-Apps
		* Backend-apps
	* Data Storage
	
* Dev
	* VPC
	* Services
		* Web-Apps
		* Backend-apps
	* Data Storage

* MGMT
	* VPC
	* Services
	* Data Storage
	
At the top level, we have separate folders for each “environment.”Within each environment, 
we have separate folders for each “component.” The components differ for every project, 
but the typical ones are:

* **VPC**: the network topology for this environment.
* **Services**: the apps or microservices to run in this environment, frontend or backend. 
	Each app lives in its own folder so it’s isolated from the others.
* **Data-storage**: the data stores to run in this environment, such as MySQL. Each data 
	store lives in its own folder so it’s isolated from the others.
	
Within each component, we have the actual Terraform templates, which we organize according 
to the following naming conventions:

* vars.tf: input variables.
* outputs.tf: output variables.
* main.tf: the actual resources.