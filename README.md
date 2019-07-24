Ansible Role: lab-build-an-operator
[![Build Status](https://travis-ci.org/openshiftlabs/openshift-homeroom-labs-deployer-ansible-role.svg?branch=master)](https://travis-ci.org/openshiftlabs/openshift-homeroom-labs-deployer-ansible-role)
=========

Ansible Role for deploying the Lab "Build an Operator" on OpenShift

Role Variables
------------

|Variable                 | Default Value     | Description   |
|-------------------------|-------------------|---------------|
|`lab_repo`               |  | *REQUIRED* Git url for the workshop content |
|`lab_branch`             | master | Git branch for the workshop |
|`lab_description`        |  | *REQUIRED* Description of the lab to show in the homeroom tile |
|`workshop_image_name`    |  | *REQUIRED* Full image location for the workshop (e.g. quay.io/openshiftlabs/lab-build-an-operator) excluding the version|
|`workshop_image_version` |  | *REQUIRED* Version of the workshop image |
|`project_name`           | openshift-labs | Name of the project where the workshops will be deployed. If the project does not exist, it will create it |
|`lab_resource_budget`    | medium | T-Shirt size (small, medium, large, x-large, xx-large) for the resources to be allocated to the workshop's pods |
|`lab_workshop_envvars`   |  | Additional environment variables to be provided to the workshop |
|`lab_terminal_envvars`   |  | Additional environment variables to be provided to the terminal |
|`lab_gateway_envvars`    |  | Additional environment variables to be provided to the gateway |
|`lab_console_version`    | 4.1.0 | Version of the OpenShift console to use in the workshop |
|`lab_idle_timeout`       | 1800 | Maximum time (in seconds) a session can be idled |
|`lab_max_session_age`    | 14400 | Maximum time (in seconds) a session can live. After this time it will be deleted no matter what |
|`lab_lets_encrypt`       | false | Does the workshop require a secured route via Let's Encrypt |
|`lab_jupyterhub_config`  |  | Additional configuration to be provided to the spawner |
|`homeroom_version`       | 1.4.0 | Version of *workshop-homeroom* image to use |
|`homeroom_template_file` | "production.json" | Homeroom template to use |
|`spawner_version`        | 4.2.0 | Version of *workshop-spawner* image to use |
|`spawner_template_file`  | learning-portal-production.json | Spawner template to use |
|`dashboard_version`      | 3.6.1 | Version of *workshop-dashboard* image to use |
|`console_image_version`  | latest | Version of *openshift-console* image to use |

OpenShift Version Compatibility
------------

When listing this role, make sure to pin the version of the role via one of the tags:

```yaml
- src: openshift_labs.homeroom_labs_deployer
  version: 1.0.0
  name: homeroom_labs_deployer
```  

The following tables shows the version combinations that are tested and verified:

|Role Version  | OpenShift Versions        |
|--------------|---------------------------|
| 1.0.x        |    3.11.x, 4.1.x, 4.2.x   |

Note that if a version combination is not listed above, it does NOT mean that it won't work on that version. The above table is merely the combinations that we have verified and tested.

Example Playbook
------------

```yaml
name: Example Playbook
hosts: localhost
connection: local
tasks:
- import_role:
    name: openshift_labs.homeroom_labs_deployer
  vars:
    project_name: "homeroom-labs"
    lab_repo: "https://github.com/openshift-labs/lab-build-an-operator"
    lab_description: "Build an Operator"
    workshop_image_name: "quay.io/openshiftlabs/lab-build-an-operator"
    workshop_version: "master"
```
