# Ansible Role: Diamond

Ansible role that installs the Diamond metric collector daemon on RedHat/CentOS.

## Requirements

- Graphite和Influxdb不能同时启用，只能二选一.
- 启用Influxdb必须安装EPEL源或是其他可以安装python-pip的源.

## Role Variables

### `defaults/main.yml`
*default lower priority variables for this role*

* `diamond_enabled: true`
* `diamond_rpm: "http://url/diamond-4.0.198-0.noarch.rpm"`

* `diamond_collectors_config_path: /etc/diamond/collectors/`
* `diamond_handlers_config_path: /etc/diamond/handlers/`

* `diamond_conf_path_prefix: "servers"`
* `diamond_conf_interval: 300`

* `diamond_graphite_handler:`
    ```
      enable: false
      host: "127.0.0.1"
      port: 2003
      timeout: 15
      batch: 1
    ```

* `diamond_influxdb_name: "influxdb"`
* `diamond_influxdb_handler:`
    ```
      enable: false
      hostname: "localhost"
      port: 8096
      database: "graphite"
      username: "root"
      password: "root"
      batch_size: 1
      cache_size: 20000
      time_precision: "s"
    ```

* `diamond_collectors_extra: {}`

## Dependencies

None

## Example Playbook

    - name: Install and configure diamond
      hosts: servers
      vars:
        diamond_rpm: "http://127.0.0.1/diamond-4.0.198-0.noarch.rpm"
        diamond_conf_path_prefix: "PRODUCTION"
        diamond_conf_interval: 30
        diamond_graphite_handler:
          enable: true
          host: "127.0.0.1"
          port: 2003
          timeout: 15
          batch: 1
        diamond_collectors_extra:
          UptimeCollector:
            enabled: True
      roles:
        - diamond

    - name: Install and configure diamond
      hosts: servers
      vars:
        diamond_rpm: "http://127.0.0.1/diamond-4.0.198-0.noarch.rpm"
        diamond_conf_path_prefix: "PRODUCTION"
        diamond_conf_interval: 30
        diamond_influxdb_name: "influxdb"
        diamond_influxdb_handler:
          enable: true
          hostname: "localhost"
          port: 8096
          database: "graphite"
          username: "root"
          password: "root"
          batch_size: 1
          cache_size: 20000
          time_precision: "s"
        diamond_collectors_extra:
          UptimeCollector:
            enabled: True
      roles:
        - diamond

## License

MIT / BSD

## Author Information

z@zstack.net
