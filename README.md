# Guacamole Server Deployment for SAML Integrastion with NGINX NPM and MariaDB on Ubuntu

This guide will walk you through deploying an Apache Guacamole Server using NGINX Proxy Manager (NPM) and MariaDB as the backend database. The deployment will use Docker containers on an Ubuntu system.

---

## Prerequisites

1. **Ubuntu System**: Ensure you have a running Ubuntu 24.04 LTS instance.
2. **Docker and Docker-Compose**: Install Docker and Docker Compose on your system.
   - Install Docker: [[Docker Installation Guide](https://docs.docker.com/engine/install/ubuntu/)]
   - https://docs.docker.com/engine/install/linux-postinstall/
3. **.env File**: Ensure you have created the `.env` file with the required variables.
4. **docker-compose.yml File**: Ensure you have the `docker-compose.yml` file with the service definitions for Guacamole, MariaDB, and NGINX Proxy Manager.

---

## Files Setup

### 1. `.env` File

Create a `.env` file with the following variables:

Update the values as per your requirements.

### 2. `docker-compose.yml` File

Please refer to the file in repository

### 3. Start Docker Containers

Run the following command to start the services:

```bash
docker compose up -d
```
### 4 Initialize the database

Install mysql Client in the MariaDB Container**

Access the guacamole-db container:

docker exec -it guacamole_db bash

Install the mysql client:

apt-get update

apt-get install mysql-client -y

Exit the container:

Import the database schema:

docker exec -i guacamole /opt/guacamole/bin/initdb.sh --mysql > initdb.sql

docker exec -i guacamole_db mysql -u root -proot_password guacamole_db < initdb.sql

### 5 Download and place SAML jar file

wget https://apache.org/dyn/closer.lua/guacamole/1.5.5/binary/guacamole-auth-sso-1.5.5.tar.gz?action=download -O guacamole-auth-sso-1.5.5.tar.gz

tar -xzf guacamole-auth-sso-1.5.5.tar.gz

sudo mkdir /home/ubuntu/guacamole-setup/guacamole/extensions/

sudo cp ./guacamole-auth-sso-1.5.5/saml/guacamole-auth-sso-saml-1.5.5.jar /home/ubuntu/guacamole-setup/guacamole/extensions/

### 6. Configure NGINX Proxy Manager

1. Access NGINX Proxy Manager at `http://<your-server-ip>:81`.
2. Log in using the default credentials (`admin@example.com` / `changeme`) and change the password.
3. Add a new proxy host:
   - **Domain Names**: Add your domain or subdomain.
   - **Forward Hostname/IP**: `guacamole`
   - **Forward Port**: `8080`
   - Enable SSL and configure certificates.

### 7. Access Guacamole

Once the NGINX Proxy Manager configuration is complete, access the Guacamole server at your domain:

```
https://<your-domain>
```

---

## Managing Containers

### Stop Containers

```bash
docker-compose down
```

### Restart Containers

```bash
docker-compose restart
```

---

## Troubleshooting

- **Logs**: Check logs for debugging using:
  ```bash
  docker-compose logs -f <service-name>
  ```
- **Database Issues**: Ensure the `.env` file has the correct database credentials.
- **Network Issues**: Verify the domain resolves to your server's IP.

---

## References

- [Apache Guacamole Documentation](https://guacamole.apache.org/)
- [MariaDB Documentation](https://mariadb.com/kb/en/documentation/)
- [NGINX Proxy Manager Documentation](https://nginxproxymanager.com/)

---

## License

This project is licensed under the MIT License. See the LICENSE file for details.

