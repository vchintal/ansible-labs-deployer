Ansible Role: homeroom_labs_deployer
[![Build Status](https://travis-ci.org/openshifthomeroom/ansible-labs-deployer.svg?branch=master)](https://travis-ci.org/openshifthomeroom/ansible-labs-deployer)
=========

Ansible Role for deploying "Homeroom labs" on OpenShift.

__NOTE__: Despite the fact that this repository is named `ansible-lab-deployer` the role is named `homeroom_labs_deployer`.

Role Variables
------------

|Variable                 | Default Value     | Description   |
|-------------------------|-------------------|---------------|
|`lab_repo`               |  | *REQUIRED* Git url for the workshop content |
|`lab_branch`             | master | *REQUIRED* Git branch for the workshop |
|`install_homeroom`       | true | Should it install the homeroom application |
|`project_name`           | openshift-homeroom | Name of the project where the workshops will be deployed. If the project does not exist, it will create it |
|`homeroom_app_name`      | homeroom | Name of the resources generated for homeroom |
|`lab_workshop_app_name`  |  | Name of the generated k8s resources for the workshop |
|`lab_workshop_name`        |  | Name of the lab to show in the homeroom tile |
|`lab_workshop_description` |  | Description for the lab to show in the homeroom tile |
|`lab_workshop_image`     |  | Full image location for the workshop (e.g. quay.io/openshiftlabs/lab-build-an-operator:v1.0.0). It should include the version|
|`lab_workshop_file`        | `workshop.yaml` | Name of the workshop definition file to use (in lab_repo/workshop directory) |
|`lab_spawner_repo`     | | Location of *workshop-spawner* repository to use |
|`lab_spawner_version`  | | Tag of *workshop-spawner* repository to use |
|`lab_spawner_image`    | | *workshop-spawner* image to use |
|`lab_spawner_mode`     | | Spawner mode to use (learning-portal, hosted-workshop, terminal-server, user-workspace, jumpbox-server) |
|`lab_spawner_variant`  | | Variant of spawner to use (production,development) |
|`lab_dashboard_repo`      |  | Location of *workshop-dashboard* repository to use |
|`lab_dashboard_version`   |  | Tag of *workshop-dashboard* repository to use |
|`lab_dashboard_image`     |  | *workshop-dashboard* image to use |
|`lab_dashboard_variant`   |  | Variant of dashboard to use (production,development) |
|`lab_terminal_image`     |  | *workshop-terminal* image to use |
|`lab_resource_budget`    | medium | T-Shirt size (small, medium, large, x-large, xx-large) for the resources to be allocated to the workshop's pods |
|`lab_max_session`        | 14400 | Maximum time (in seconds) a session can live. After this time it will be deleted no matter what |
|`lab_idle_timeout`       | 1800 | Maximum time (in seconds) a session can be idled |
|`lab_server_limit`       | | Number of maximum sessions per node |
|`lab_lets_encrypt`       | false | Does the workshop require a secured route via Let's Encrypt |
|`settings`               | | xxx |
|`lab_workshop_file`      | | xxx |
|`lab_workshop_memory`    | | xxx |
|`lab_spawner_role`       | | xxx |
|`lab_spawner_password`   | | xxx |
|`lab_download_url`       | | xxx |
|`lab_console_image`      | | Image of the OpenShift console to use in the workshop (fully quialified)|
|`lab_workshop_envvars`   |  | Additional environment variables to be provided to the workshop |
|`lab_terminal_envvars`   |  | Additional environment variables to be provided to the terminal |
|`lab_gateway_envvars`    |  | Additional environment variables to be provided to the gateway |
|`lab_jupyterhub_config`  |  | Additional configuration to be provided to the spawner |

## Running an example

Install the role:

```bash
ansible-galaxy install openshift_labs.homeroom_labs_deployer
```

And now create your playbook (my-playbook.yml):

```yaml
- name: Example Playbook
  hosts: localhost
  connection: local
  tasks:
  - import_role:
      name: homeroom_labs_deployer
    vars:
      lab_repo: "https://github.com/openshift-labs/lab-build-an-operator"
      lab_workshop_name: "Build an operator"
      lab_workshop_description: "Automate applications with k8s operators"
```

And execute it:

```bash
ansible-playbook my-playbook.yml
```

### Use a requirements file

Create a requirements file (requirements.yml) to install the role:

```yaml
- src: openshift_labs.homeroom_labs_deployer
  version: master
  name: homeroom_labs_deployer
```

Install the role:

```bash
ansible-galaxy install -r requirements.yml -f
```

## Use meta/main.yaml

_NOTE_: this is kind of a hack to use this within a role

Add the dependency to your role including it in the `dependencies` section of your meta. This will execute that role with the provided vars.

```yaml
dependencies:
  - role: openshift_labs.homeroom_labs_deployer
    version: master
    name: homeroom_labs_deployer
    vars:
      lab_repo: "https://github.com/openshift-labs/lab-build-an-operator"
      lab_workshop_description: "Automate applications with k8s operators"
```

## More examples

Here are some other examples with alernate configuratiosn:

```yaml
- name: Event specific configuration
  hosts: localhost
  connection: local
  tasks:
  - import_role:
      name: homeroom_labs_deployer
    vars:
      lab_repo: "https://github.com/openshift-labs/lab-build-an-operator"
      lab_branch: "rhte19"
      event: "rhte19"
      lab_workshop_description: "Automate applications with k8s operators"
```


## OpenShift Version Compatibility

When listing this role, make sure to pin the version of the role via one of the tags:

```yaml
- src: openshift_labs.homeroom_labs_deployer
  version: 1.0.0
  name: homeroom_labs_deployer
```

__NOTE__: This should be set in a requirements.yml file and installed via `ansible-galaxy install -r requirements.yml -f`

The following tables shows the version combinations that are tested and verified:

|Role Version  | OpenShift Versions        |
|--------------|---------------------------|
| 1.0.x        |    3.11.x, 4.1.x, 4.2.x   |

Note that if a version combination is not listed above, it does NOT mean that it won't work on that version. The above table is merely the combinations that we have verified and tested.
