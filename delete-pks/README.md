If you installed PKS using the steps at https://github.com/pivotal-alexis-villalon/alexis-pks-pipelines/blob/master/README.md, then you can create a delete-pks pipeline using the delete-pks/pipeline.yml.  Just use both pipeline.yml and params.yml from delete-pks/ and then edit params.yml to match your environment.  

Then, create a delete-pks pipeline by running:<br>
`$ fly -t <concourse-env> sp -p delete-pks -c pipeline.yml -l params.yml`

Unpause it:<br>
`$ fly -t <concourse-env> up -p delete-pks`

Then, in Concourse UI, manually trigger the delete-installation job.  

Finally, once delete-installation job is successful, manually trigger the delete-vm job.
