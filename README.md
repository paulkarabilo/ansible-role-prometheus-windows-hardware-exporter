# ansible-role-prometheus-windows-hardware-exporter

**Prometheus Windows Hardware Exporter** Ansible role to install and configure the Prometheus Windows Hardware Exporter service on Windows hosts.

## Overview

This role downloads and installs the PrometheusWindowsHardwareExporter MSI, ensures the Windows service is created and configured with the requested startup type and listen address, and exposes configuration variables to control installation, metrics collection, firewall settings, and an optional PawnIO helper install.

> Note: This role is intended for Windows managed nodes and uses the `ansible.windows` collection modules (`win_get_url`, `win_package`, `win_service`, etc.).

## Requirements

- Ansible 2.9+ (or newer) and the `ansible.windows` collection available on the controller.
- Windows hosts capable of installing MSI packages and running services.

You can install the required collection with:

```bash
ansible-galaxy collection install ansible.windows
```

## Role Variables

All variables are defined in `defaults/main.yml`. Commonly-used variables are:

| Variable | Default | Description |
||||
| `prometheus_windows_hardware_exporter_version` | `"0.0.3"` | Exporter version to download |
| `prometheus_windows_hardware_exporter_full_download_url` | `computed` | Full URL to MSI download |
| `prometheus_windows_hardware_exporter_package_name` | `PrometheusWindowsHardwareExporter.msi` | MSI file name |
| `prometheus_windows_hardware_exporter_sha256_checksum` | (sha256) | Checksum used to verify the download |
| `prometheus_windows_hardware_exporter_install_arguments` | `/quiet` | Arguments passed to MSI installer |
| `prometheus_windows_hardware_exporter_install_path` | `C:\Program Files\PrometheusWindowsHardwareExporter` | Expected install path |
| `prometheus_windows_hardware_exporter_service_name` | `PrometheusWindowsHardwareExporter` | Windows service name |
| `prometheus_windows_hardware_exporter_listen_address` | `http://+:9182/` | Address exporter listens on |
| `prometheus_windows_hardware_exporter_startup_type` | `auto` | Service startup type (e.g., `auto`, `manual`, `disabled`) |
| `prometheus_windows_hardware_exporter_metrics` | `[cpu, memory, disk, network, gpu]` | List of enabled collectors |
| `prometheus_windows_hardware_exporter_firewall_*` | See defaults | Firewall rule controls (name, profiles, state, etc.) |
| `prometheus_windows_hardware_exporter_install_pawnio` | `true` | Whether to install PawnIO helper |
| `prometheus_windows_hardware_exporter_pawnio_version` | `2.0.1` | PawnIO version |

## Installing role from GitHub

- Install with ansible-galaxy from a git source:

```bash
ansible-galaxy role install git+https://github.com/paulkarabilo/ansible-role-prometheus-windows-hardware-exporter.git
```

- Or add to `requirements.yml` for automated installs:

```yaml
- src: git+https://github.com/paulkarabilo/ansible-role-prometheus-windows-hardware-exporter.git
  name: prometheus_windows_hardware_exporter
```

## Example Playbook

Basic usage to install the exporter and configure defaults:

```yaml
- hosts: windows
  roles:
    - role: prometheus_windows_hardware_exporter
      vars:
        prometheus_windows_hardware_exporter_listen_address: "http://+:9182/"
        prometheus_windows_hardware_exporter_metrics:
          - cpu
          - memory
          - disk
```

## License

This role is distributed under the terms in the [LICENSE](./LICENSE) file.

