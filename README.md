# Webarchitects RabbitMQ Ansible Role

[![pipeline status](https://git.coop/webarch/rabbitmq/badges/master/pipeline.svg)](https://git.coop/webarch/rabbitmq/-/commits/master)

This repo contains an Ansible role to install and configure RabbitMQ on Debian, it has been written spefifically for [ONLYOFFICE](https://helpcenter.onlyoffice.com/installation/docs-community-install-ubuntu.aspx).

## Role variables

See the [defaults/main.yml](defaults/main.yml) file for the default variables, the [vars/main.yml](vars/main.yml) file for the preset variables and the [meta/argument_specs.yml](meta/argument_specs.yml) file for the variable specification.

### rabbitmq

Set the `rabbitmq` variable to `true` run the tasks in this role, it defaults to `false`.

### rabbitmq_configuration

A optional dictionary of variables and values to be written to `/etc/rabbitmq/rabbitmq.conf`, see the [example conf file](https://raw.githubusercontent.com/rabbitmq/rabbitmq-server/main/deps/rabbit/docs/rabbitmq.conf.example) and the [configuration documentation](https://www.rabbitmq.com/configure.html), the default configuration limits RabbitMQ to listen for IPv4 and IPv6 connections on the `localhost`:

```yaml
rabbitmq_configuration:
  "listeners.tcp.local": "127.0.0.1:5672"
  "listeners.tcp.local_v6": "::1:5672"
```

### rabbitmq_systemd_units

A dictionary to be used with the [systemd role](https://git.coop/webarch/systemd).

### rabbitmq_users

A optional list of dictionary of users for the [community.rabbitmq.rabbitmq_user module](https://docs.ansible.com/ansible/latest/collections/community/rabbitmq/rabbitmq_user_module.html), currently this only support two list options, `username` and `state`. For users that are listed as `present` a random password will be generated and written to `/root/.rabbitmq.$USER.passwd`.

By default this role removes the `guest` role:

```yaml
rabbitmq_users:
  - username: guest
    state: absent
```

#### username

The name of the RabbitMQ user.

#### state

The state of the RabbitMQ user, `absent` or `present`.

### rabbitmq_validate

A boolean, which defaults to `true`, set `rabbitmq_validate` to `false` to skip using the [ansible.builtin.validate_argument_spec module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/validate_argument_spec_module.html) to validate variables that start with `rabbitmq_`.

## Notes

Changing the hostname of a server will caused RabbitMQ to fail, see [this thread](https://stackoverflow.com/a/31977791).

## Repository

The primary URL of this repo is [`https://git.coop/webarch/rabbitmq`](https://git.coop/webarch/rabbitmq) however it is also [mirrored to GitHub](https://github.com/webarch-coop/ansible-role-rabbitmq) and [available via Ansible Galaxy](https://galaxy.ansible.com/chriscroome/rabbitmq).

If you use this role please use a tagged release, see [the release notes](https://git.coop/webarch/rabbitmq/-/releases).

## Copyright

Copyright 2020-2023 Chris Croome, &lt;[chris@webarchitects.co.uk](mailto:chris@webarchitects.co.uk)&gt;.

This role is released under [the same terms as Ansible itself](https://github.com/ansible/ansible/blob/devel/COPYING), the [GNU GPLv3](LICENSE).
