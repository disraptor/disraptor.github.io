# 3. Deploy Discourse with Disraptor
&larr; [2. Setup a Discourse development environment (Ubuntu)](setup-a-discourse-development-environment-ubuntu.md)<br/>
&rarr; [4. Setup Disraptor](setup-disraptor.md)

---

*(based on [discourse/discourse: INSTALL-cloud.md](https://github.com/discourse/discourse/blob/master/docs/INSTALL-cloud.md) and [discourse/discourse_docker: README.md](https://github.com/discourse/discourse_docker/blob/master/README.md).)*

This document describes how to deploy Discourse with Disraptor. You will need root access to a docker-compatible 64-bit Linux server. You will also need a mail server and a domain name.



## Prerequisites

1. Get the following things ready:

   - The host name for your web application (e.g. `tira.io`). Discourse requires a domain name for deployment. An IP address is not sufficient.
   - An email address for the admin account (e.g. `info@tira.io`).
   - The SMPT server address (e.g. `smtp.tira.io`) of your mail server plus port (587), user name (e.g. `admin`), and password.

2. Connect to the server via SSH.
3. Install the following software on the server:

   - [Ruby 2.7+](https://www.ruby-lang.org/en/downloads/)
   - [Postgres 14+](https://www.postgresql.org/download/)
   - [Redis 7.0+](https://redis.io/download)

     ```sh
     sudo apt install redis-server ruby postgresql
     ```
   **Note:** This might work with older versions aswell. If you cannot get a hand on these easily try to make it work with what you have first.

   - Docker and git:

     ```sh
     sudo apt install docker.io git
     ```



## Install Discourse with Disraptor via Docker

1. Clone the Discourse Docker repository into `/var/discourse`:

   ```sh
   sudo -s
   mkdir /var/discourse
   git clone https://github.com/discourse/discourse_docker.git /var/discourse
   ```

2. Run and follow the instructions of the Discourse setup script:

   ```sh
   cd /var/discourse
   ./discourse-setup
   ```

   This produces an `app.yml` file and starts a bootstrap process.

3. Open the previously generated `app.yml` file:

   ```sh
   sudo vim containers/app.yml
   ```

   In the section containing the `hooks` key, add a command to clone the Disraptor repository as shown below:

   ```yaml
   hooks:
     after_code:
       - exec:
         cd: $home/plugins
         cmd:
           - git clone https://github.com/discourse/docker_manager.git
           - git clone https://github.com/disraptor/disraptor.git
   ```

4. Restart the bootstrap process:

   ```sh
   ./launcher rebuild app
   ```



## Troubleshooting

- List running Docker containers:

  ```sh
  docker container ls
  ```
