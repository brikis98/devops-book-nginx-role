# Ansible Nginx Role

This repo contains a simple Ansible role designed to deploy Nginx as a load balancer. This role does the following:

* Install Nginx. This step only works on Linux servers that support `yum` (e.g., Amazon Linux).
* Configure Nginx to listen on port 80.
* Configure Nginx to proxy requests to the `/` URL to the servers you pass in via the `servers` variable. 
 
This is sample code from the book and blog post series [_Fundamentals of DevOps and Software 
Delivery_](https://www.fundamentals-of-devops.com) by Yevgeniy Brikman. Note that the vast majority of the sample
code lives in another repo, https://github.com/brikis98/devops-book, and this repo only contains an Ansible role that
will work with `ansible-galaxy`. 

> [!IMPORTANT]  
> This repo contains example code for learning and experimenting only, in conjunction with the book and blog post 
> series. This code is _not_ designed for direct production usage. If you're looking for code you can use directly in
> production, check out the [Gruntwork Library](https://www.gruntwork.io/products/library).

## Quick start

Create a `requirements.txt` file with the following contents, replacing `<VERSION>` with the latest version from the
[releases page](https://github.com/brikis98/devops-book-nginx-role/releases):

```yml
- name: nginx
  src: https://github.com/brikis98/devops-book-nginx-role
  version: <VERSION>
```

Run the following command to install the role:

```console
$ ansible-galaxy role install -r requirements.yml
```

Now you can use the role in your playbooks, replacing `<SERVERS>` with the list of servers (list of IPs and ports) to
proxy:

```yml
- name: Configure servers to run nginx
  hosts: nginx_instances
  gather_facts: true
  become: true
  roles:
    - role: nginx       
      vars:
        servers: <SERVERS> 
```

For example, to proxy several known IPs, such as `1.2.3.4` and `5.6.7.8`, at port 80:

```yml
- name: Configure servers to run nginx
  hosts: nginx_instances
  gather_facts: true
  become: true
  roles:
    - role: nginx       
      vars:
        servers:
          - 1.2.3.4:80
          - 5.6.7.8:80
```

## License

This code is released under the MIT License. See [LICENSE.txt](./LICENSE.txt).
