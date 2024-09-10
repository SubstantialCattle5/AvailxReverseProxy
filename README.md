```markdown
# Avail Client Setup with Nginx Reverse Proxy

This project sets up the Avail Client and configures Nginx as a reverse proxy for the Avail Client's RPC and Prometheus endpoints. The setup is automated using Vagrant and Ansible.

## Prerequisites

Before starting, make sure you have the following installed:

- [Vagrant](https://www.vagrantup.com/downloads)
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)

## Getting Started

### 1. Clone the Repository

```bash
git clone <repository-url>
cd <repository-directory>
```

### 2. Set Environment Variables

You can customize the ports and node name by setting the following environment variables:

```bash
export RPC_PORT=9933
export PROMETHEUS_PORT=9651
export NODE_NAME=test-node
export NGINX_DOMAIN=192.168.56.101
export NGINX_HOST_PORT=8080
```

If the environment variables are not set, default values will be used.

### 3. Modify the Avail Client Download Link (if needed)

By default, the playbook downloads the Avail Client binary for **Ubuntu 22.04 (Intel x86_64)** from this link:

```bash
https://github.com/availproject/avail/releases/download/v2.2.4.1/x86_64-ubuntu-2204-avail-node.tar.gz
```

If you're using a different version of Ubuntu or a different architecture, **you must update the download link** in the Ansible playbook (`avail_revprox_setup.yml`). Refer to the [Avail releases page](https://github.com/availproject/avail/releases) for the appropriate binary.

### 4. Run the Vagrant Setup

```bash
vagrant up
```

This will create a VirtualBox virtual machine, install necessary packages, download and configure the Avail Client, and set up Nginx as a reverse proxy.

### 5. Access the Avail Node

Once the setup is complete, you can access the RPC and Prometheus endpoints:

- **RPC**: `http://<your-host-ip>:<RPC_PORT>/api`
- **Prometheus Metrics**: `http://<your-host-ip>:<PROMETHEUS_PORT>/metrics`

By default, the RPC endpoint is exposed on port `9933` and the Prometheus metrics endpoint on port `9651`.

### 6. Modify Configuration

To modify the setup, you can update the following files:

- **Vagrantfile**: Change VM specifications such as memory, CPUs, and port forwarding.
- **Ansible Playbook** (`avail_revprox_setup.yml`): Customize the installation and configuration of the Avail Client and Nginx.
- **Nginx Template** (`nginx_avail.j2`): Update the reverse proxy settings if needed.

### 7. Stopping the VM

Once you're done, you can halt the Vagrant VM using:

```bash
vagrant halt
```

To completely remove the VM:

```bash
vagrant destroy
```

## Nginx Reverse Proxy

The Nginx configuration sets up a reverse proxy to forward traffic to the Avail Client for RPC and Prometheus metrics:

- RPC requests are proxied to `http://localhost:<RPC_PORT>/api`
- Prometheus metrics requests are proxied to `http://localhost:<PROMETHEUS_PORT>/metrics`

The Nginx configuration is defined in the template file `nginx_avail.j2`.

## Disclaimer

The default setup in this repository downloads the **Avail Client binary for Ubuntu 22.04 (Intel x86_64)**. If you're using a different version of Ubuntu or a different architecture, you **must modify the download link** in the Ansible playbook (`avail_revprox_setup.yml`). Visit the [Avail project releases page](https://github.com/availproject/avail/releases) to find the correct binary for your environment.

## Troubleshooting

- **Port conflicts**: Ensure the host machine ports specified in the environment variables are not in use by other processes.
- **Vagrant issues**: If you encounter issues with Vagrant, run `vagrant destroy` and try `vagrant up` again.
- **Ansible errors**: Check the Ansible output to see if there are any missing packages or permissions issues.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
```

### Changes:
- Added a new section titled **"3. Modify the Avail Client Download Link (if needed)"** with instructions on how to change the link if the user is not on Ubuntu 22.04 (Intel x86_64).
- Added a **Disclaimer** section, explicitly informing users to update the download link if necessary.

Let me know if you'd like any further adjustments!