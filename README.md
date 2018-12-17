# alexis-pks-pipelines
This contains the pipelines I use to install, maintain and delete PKS

Step 1: Get and note down the following
get s3 bucket, access_key_id, secret_access_key and region.
get system_domain
get pks_api uri
get pivnet token
get opsman_version

Step 2: generate the following files
state.yml
env.yml
opsman.yml
auth.yml
director.yml
download-pks-config.yml
gencert-pks.yml
pks-config.yml
updated-pks-config.yml


Step 3: download the platform automation files (image and tasks)

Step 4: Upload to s3 bucket: 
- all yml files from step2
- the platform automation files from step3


Step 5: Prepare the pipeline.yml and params.yml files.
- you can use the pipeline.yml file as is.
- you need to modify the params.yml file for your usage.



Step 6: Set the pipeline and unpause it
$ fly -t cc24 sp -p install-pks -c pipeline.yml -l params.yml
$ fly -t cc24 up -p install-pks


Step 7: Go to the Concourse UI and trigger the first job (create-opsman-vm)


Step 8: Relax, sit back and watch it fly to completion.  ETA 50 mins.  

