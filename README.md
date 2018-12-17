# alexis-pks-pipelines
This contains the pipelines I use to install and delete PKS foundations.


Step 1: Get and note down the following.  You will use these in creating your params.yml file for step4.

- s3 bucket, access_key_id, secret_access_key and region
	- get these from your S3 environment
- system_domain
- pks_api uri
- pivnet legacy token
- opsman version


Step 2: generate the following files:

- state.yml
    generate an empty file.  `touch state.yml`
- env.yml
    see files/env.yml.  This file contains all credentials needed by the om cli tool to log on to Ops Man API.
- opsman.yml
    see files/opsman.yml.  This contains all information needed by the p-automator tool to communicate with the IaaS to create and spin up that Ops Man VM.
- auth.yml
    see files/auth.yml.  This contains all information needed by the om cli tool to initially configure the authentication that you want Ops Man to use.
- director.yml
    see files/director.yml.  This contains all configuration information for BOSH Director.  See https://docs.pivotal.io/pcf-automation/release/configuration-management/configure-director.html on how to generate one from a running config.
- download-pks-config.yml
    see files/download-pks-config.yml.  This contains information on what PKS tile to download from Pivnet.
- gencert-pks.yml
    see tasks/gencert-pks.yml.  This is a task file to help generate the Server certificate and key for the PKS API.  The certificate and key will be created and signed using the Ops Man Root CA.
- pks-config.yml
    see files/pks-config.yml.  This contains all the configuration information for the PKS Tile.  See https://docs.pivotal.io/pcf-automation/release/configuration-management/configure-product.html on how to generate one from a running config.

In each of the above files, there are placeholders in the string format of '((value_placeholder))' that you need to update.  The only exception are ((pks_tls.cert_pem)) and ((pks_tls.private_key_pem)) in pks.config.yml, which will be taken care of by the gencert-pks task, so leave these 2 untouched.  There are also 'changeme' strings that you need to, well, change.


Step 3: download the platform automation files (image and tasks) from pivnet.


Step 4: Upload to your s3 bucket: 
- all yml files from step2
- the platform automation files from step3


Step 5: Prepare the pipeline.yml and params.yml files.
- you can use the pipeline.yml file as is.
- you need to modify the params.yml file for your usage.



Step 6: Set the pipeline and unpause it
$ fly -t <concourse env> sp -p install-pks -c pipeline.yml -l params.yml
$ fly -t <concourse env> up -p install-pks


Step 7: Go to the Concourse UI and trigger the first job (create-opsman-vm)


Step 8: Relax, sit back and watch it fly to completion.  ETA 50 mins.  Once completed you will have a PKS foundation (with Ops Man, Director and PKS VMs).

