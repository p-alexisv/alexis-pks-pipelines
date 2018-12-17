# alexis-pks-pipelines
This contains the pipelines I use to install and delete PKS foundations.



<h1>Set up and run an install-pks pipeline</h1>

- This is to automate installation of the whole foundation consisting of Ops Man, BOSH Director and PKS VM's.


<h2>Step 1: Get and note down the following.  You will use these in creating your params.yml file for step4.</h2>

- s3 bucket, access_key_id, secret_access_key and region
	- get these from your S3 environment
- system_domain
- pks_api uri
- pivnet legacy token
- opsman version

<h2>Step 2: Generate / prepare the following files:</h2>

- state.yml
    - generate an empty file (e.g.,`touch state.yml`)
- env.yml
    - see files/env.yml.  This file contains all the credentials needed by the om cli tool to log on to the Ops Man API.
- opsman.yml
    - see files/opsman.yml.  This contains all the information needed by the p-automator tool (platform automation tool already installed in the platform automation image) to communicate with the IaaS to create and spin up that Ops Man VM.
- auth.yml
    - see files/auth.yml.  This contains all the information needed by the om cli tool to initially configure the authentication that you want Ops Man to use.
- director.yml
    - see files/director.yml.  This contains all the settings information needed to configure the BOSH Director.  See https://docs.pivotal.io/pcf-automation/release/configuration-management/configure-director.html on how to generate one from a running config.
- download-pks-config.yml
    - see files/download-pks-config.yml.  This contains the details of the PKS tile to download from Pivnet.
- gencert-pks.yml
    - see tasks/gencert-pks.yml.  This is a task file to help generate the Server certificate and key for the PKS API.  The certificate and key will be created and signed using the Ops Man Root CA.  Download this file from this repo.
- pks-config.yml
    - see files/pks-config.yml.  This contains all the settings information needed to configure the PKS Tile.  See https://docs.pivotal.io/pcf-automation/release/configuration-management/configure-product.html on how to generate one from a running config.

In each of the above files, there are placeholders in the string format of '((value_placeholder))' that you need to update.  The only exception are ((pks_tls.cert_pem)) and ((pks_tls.private_key_pem)) in pks.config.yml, which will be taken care of by the gencert-pks task, so leave these 2 untouched.  There are also 'changeme' strings that you need to, well, change.


<h2>Step 3: Download the platform automation files (image and tasks) from pivnet.</h2>

- Download them at https://network.pivotal.io/products/platform-automation/


<h2>Step 4: Upload to your s3 bucket: </h2>

- all yml files from step2
- the platform automation files from step3


<h2>Step 5: Prepare the pipeline.yml and params.yml files.</h2>

- You can download and use the pipeline.yml file as is.  See install-pks/pipeline.yml
- You need to modify the params.yml file for your usage.  See install-pks/params.yml


<h2>Step 6: Set the pipeline and unpause it</h2>

- $ fly -t <concourse env> sp -p install-pks -c pipeline.yml -l params.yml
- $ fly -t <concourse env> up -p install-pks


<h2>Step 7: Go to the Concourse UI and trigger the first job (create-opsman-vm)</h2>


<h2>Step 8: Relax, sit back and watch it fly to completion.  ETA 50 mins.  Once completed you will have a PKS foundation (with Ops Man, Director and PKS VMs).</h2>
