# cf-deployment-concourse-tasks

<The way this document looks in the raw
is intentional.
For a discussion of the intention,
Please see
https://github.com/JesseTAlford/texts/blob/master/the-art-of-markdown.md>

This repo contains concourse tasks for use with [cf-deployment][cf-deployment-repo].
If you are trying to deploy to GCP, you may wish to check the [GCP Deployment Guide][gcp-deployment-guide].
Each task is in its own directory.
A task will generally be composed of a task.yml to be referenced in pipelines,
a supporting task file, and a Dockerfile.
The Dockerfile is built and pushed to Dockerhub regularly
in CI maintained by the CF Release Integration team [here][runtime-ci-build-docker-images].

It should be clear how to use each task
from the task.yml
and the description below.
If you find that it is not,
please contact the Release Integration team
in our [Slack channel dedicated to supporting users of `cf-deployment`][cf-deployment-slack-channel].
Alternatively, you can [open an issue][issues-page].

## Tasks
Tasks are listed here alphabetically,
along with a brief description
meant to be used alongside the `task.yml` within each task directory
to understand the tasks'
purpose, interface, and options.
Each title is also a link
to the appropriate task.yml.

### [bbl-destroy][bbl-destroy-task-yaml]
This destroys the director
and infrastructure
created by [bbl](https://github.com/cloudfoundry/bosh-bootloader).
Debug output
is written to
bbl_destroy.txt
to help debug failures
in this task.

### [bbl-up][bbl-up-task-yaml]
This uses [bbl](https://github.com/cloudfoundry/bosh-bootloader)
to create your infrastructure
and deploy a BOSH director.
Debug output
is written to
bbl_up.txt,
bbl_create_lbs.txt,
and bbl_update_lbs.txt
to help debug failures
in this task.
This task requires
a certificate and key
which can be generated using
the commands specified [here][gcp-deployment-guide-on-certificates].

### [bosh-deploy][bosh-deploy-task-yaml]
This performs a BOSH deployment
and outputs a vars-store.
Optionally, operations files may be applied
to the deployment manifest.

It's also configurable to
regenerate the vars-store
on each deployment
though this is not the default behavior.
This is helpful for testing
changes to variable generation,
but is only expected to work
with fresh deployments.

### [bosh-deploy-with-created-release][bosh-deploy-with-created-release-task-yaml]
This creates and applies an
additional operations file to `cf-deployment.yml`,
which causes BOSH to
create, upload, and use a dev release
from the provided release folder
in place of the version specified in `cf-deployment.yml`.
This is useful for testing an upstream component.
Otherwise identical to the `bosh-deploy` task above.

### [bosh-upload-stemcell-from-cf-deployment][bosh-upload-stemcell-from-cf-deployment-task-yaml]
This uploads the stemcell version
specified in `cf-deployment`
to the BOSH director.

### [set-feature-flags][set-feature-flags-task-yaml]
This will
toggle
the specified feature-flags
based on their boolean values.

### [update-integration-configs][update-integration-configs-task-yaml]
This updates integration files
to be consumed by CATs and RATs
with credentials drawn from
a vars-store.

[bbl-destroy-task-yaml]: https://github.com/cloudfoundry/cf-deployment-concourse-tasks/blob/master/bbl-destroy/task.yml
[bbl-up-task-yaml]: https://github.com/cloudfoundry/cf-deployment-concourse-tasks/blob/master/bbl-up/task.yml
[bosh-deploy-task-yaml]: https://github.com/cloudfoundry/cf-deployment-concourse-tasks/blob/master/bosh-deploy/task.yml
[bosh-deploy-with-created-release-task-yaml]: https://github.com/cloudfoundry/cf-deployment-concourse-tasks/blob/master/bosh-deploy-with-created-release/task.yml
[bosh-upload-stemcell-from-cf-deployment-task-yaml]: https://github.com/cloudfoundry/cf-deployment-concourse-tasks/blob/master/bosh-upload-stemcell-from-cf-deployment/task.yml
[cf-deployment-repo]: https://github.com/cloudfoundry/cf-deployment
[cf-deployment-slack-channel]: https://cloudfoundry.slack.com/messages/cf-deployment/
[deploy-with-created-lines]: https://github.com/cloudfoundry/cf-deployment-concourse-tasks/blob/master/bosh-deploy-with-created-release/task#L49-L55
[issues-page]: https://github.com/cloudfoundry/cf-deployment-concourse-tasks/issues
[set-feature-flags-task-yaml]: https://github.com/cloudfoundry/cf-deployment-concourse-tasks/blob/master/set-feature-flags/task.yml
[runtime-ci-build-docker-images]: https://runtime.ci.cf-app.com/teams/main/pipelines/build-docker-images?groups=cf-deployment-concourse-tasks
[update-integration-configs-task-yaml]: https://github.com/cloudfoundry/cf-deployment-concourse-tasks/blob/master/update-integration-configs/task.yml
[gcp-deployment-guide]: https://github.com/cloudfoundry/cf-deployment/blob/develop/gcp-deployment-guide.md
[gcp-deployment-guide-on-certificates]: https://github.com/cloudfoundry/cf-deployment/blob/develop/gcp-deployment-guide.md#on-certificates
