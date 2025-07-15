# 🐋 Two-Tier Flask App with MySQL using Docker

This project sets up a Flask web application with a MySQL database using Docker and Docker Compose. The Flask app connects to MySQL using environment variables, and both containers run on a custom Docker network.

---

## 🛠️ Steps to Run

```bash
# Clone the repository
git clone https://github.com/LondheShubham153/two-tier-flask-app.git
cd two-tier-flask-app/

# (Optional) Rebuild Dockerfile
vim Dockerfile
docker build . -t flaskapp

# Create a custom Docker network
docker network create twotier

# Run MySQL container
docker run -d --name mysql --network=twotier -p 3306:3306 \
  -e MYSQL_ROOT_PASSWORD=admin \
  -e MYSQL_USER=admin \
  -e MYSQL_PASSWORD=admin \
  -e MYSQL_DATABASE=mydb \
  mysql:5.7

# Run Flask app container
docker run -d --name flaskapp --network=twotier -p 5000:5000 \
  -e MYSQL_HOST=mysql \
  -e MYSQL_USER=admin \
  -e MYSQL_PASSWORD=admin \
  -e MYSQL_DB=mydb \
  pritamcalsoft/flaskapp:latest

# Access the app
http://localhost:5000
```
## 🛠️ Errors encountered

MySQLdb.OperationalError: (2002, "Can't connect to server on 'mysql' (115)")

Cause: The Flask app started before MySQL was ready.

Fix:

If not using docker-compose.yml then 
- 1st start the mysql container then after 10s start the flaskapp
- name the mysql container as mysql and use the same name in MYSQL_HOST for the flaskapp

Use depends_on in docker-compose.yml

Ensure both containers are on the same custom network (twotier)

Use healthchecks or delay initialization in Flask
