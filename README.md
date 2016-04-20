# Ansible role `tomcat`

An Ansible role for setting up Tomcat on RHEL/CentOS 7. Specifically, the responsibilities of this role are to:

- Install packages (from the EPEL repository)
- Manage configuration

The firewall configuration is not a concern of this role. Use another role for that (e.g. [bertvv.el7](https://galaxy.ansible.com/bertvv/el7))

## Requirements

No specific requirements

## Role Variables


| Variable                      | Required | Default     | Comments                                                           |
| :---                          | :---     | :---        | :---                                                               |
| `tomcat_port`                 | no       | 8080        | The port number for the Tomcat service                             |
| `tomcat_libraries`            | no       | []          | List of libraries (.jar files) to install in Tomcat lib/ directory |
| `tomcat_deploy_wars`          | no       | []          | List of application archives (.war files) to be deployed           |
| `tomcat_install_admin_webapp` | no       | false       | When true, the admin web application is installed                  |
| `tomcat_roles`                | no       | (see below) | List of user roles to be defined in tomcat-users.xml (see below)   |
| `tomcat_users`                | no       | []          | List of users to be defined in tomcat-users.xml (see below)        |

### Users and roles

When installing the Manager web application, you also need to define at least one user and some roles. See the [Tomcat Documentation](https://tomcat.apache.org/tomcat-7.0-doc/manager-howto.html#Configuring_Manager_Application_Access) for details. The following roles are available by default:

```Yaml
tomcat_roles:
  - manager-gui
  - manager-status
  - manager-script
  - manager-jmx
```

You can override these by redefining `tomcat_roles` in your `host_vars` or `group_vars`.

To enable access to the Manager web application, you must create a username/password and associate one of the `manager-xxx` roles with it. The `tomcat_users` variable takes care of this, e.g.:

```Yaml
tomcat_users:
  - name: admin
    password: 'Boavtug8'
    roles:
      - manager-gui
  - name: john
    password: 'yirphUr7'
    roles:
      - manager-jmx
```

## Dependencies

No dependencies.

## Example Playbook

See the [test playbook](tests/test.yml)

## Testing

Tests for this role are provided in the form of a Vagrant environment that is kept in a separate branch, `tests`. I use [git-worktree(1)](https://git-scm.com/docs/git-worktree) to include the test code into the working directory. Instructions for running the tests:

1. Fetch the tests branch: `git fetch origin tests`
2. Create a Git worktree for the test code: `git worktree add tests tests` (remark: this requires at least Git v2.5.0). This will create a directory `tests/`.
3. `cd tests/`
4. `vagrant up` will then create a VM and apply a test playbook (`test.yml`).
5. Point your browser to <http://192.168.56.7:8084/sample/> to view the sample application that was deployed on the server by the test playbook.

You may want to change the base box into one that you like. The current one, [bertvv/centos72](https://atlas.hashicorp.com/bertvv/boxes/centos72) was generated using a Packer template from the [Boxcutter project](https://github.com/boxcutter/centos) with a few modifications.

## Contributing

Issues, feature requests, ideas are appreciated and can be posted in the Issues section. Pull requests are also very welcome. Preferably, create a topic branch and when submitting, squash your commits into one (with a descriptive message).

## License

BSD

## Author Information

Bert Van Vreckem (bert.vanvreckem@gmail.com)

