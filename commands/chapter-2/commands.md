- Search `podman` for images: `sudo podman search rhel`
- Download an image using the pull commands: `sudo podman pull rhel`. This downloads and saves `rhel` image locally for future use
- List the images stored locally: `sudo podman images`
- Running the images: `sudo podman run rhel7:7.5 echo "Hello World!`
- To run in the background use `-d` flag. 
- You can use `podman inspect` to get attributes like the IP address, image being used etc: `sudo podman inspect -l \`
- `--name`: name all of your containers to organize better instead of using the ID
- `-it`: interactive shell flag. often used in conjunction: `sudo podman run -it rhel7:7.5 /bin/bash`
- passing information to containers, use environment variables using `-e` flag: `sudo podman run --name mysql_custom -e MYSQL_USER=admin -e MYSQL_PWD=admin_password -d \
rhmap47/mysql:5.5`

### Creating a MY_SQL container instance

`lab container-create start`

Starting a container from my sql image. Creates a DB called items and sets it root password too.
```
sudo podman run --name mysql-basic \
> -e MYSQL_USER=user1 -e MYSQL_PASSWORD=mypa55 \
> -e MYSQL_DATABASE=items -e MYSQL_ROOT_PASSWORD=r00tpa55 \
> -d rhscl/mysql-57-rhel7:5.7-3.14
```

Verifying container started without any issues
```
sudo podman ps --format "{{.ID}} {{.Image}} {{.Names}}"
```

Access the sandbox using the following command:

```
sudo podman exec -it mysql-basic /bin/bash
bash-4.2$
```

Adding data to the database:
- connect to MySQL as db admin: `mysql -uroot`
- `show databases;`
- `use items;`
- Create a table called Projects in the items database
```
CREATE TABLE Projects (id int(11) NOT NULL,
-> name varchar(255) DEFAULT NULL,
-> code varchar(255) DEFAULT NULL,
-> PRIMARY KEY (id));
```
- `show tables;`
- `insert into Projects (id, name, code) values (1, 'IOT', 'PX001');`
- `insert into Projects (id, name, code) values (2,'DevOps','DO180');`
- `select * from Projects;`
- `exit`
- `lab container-create finish`


### Lab: 
- `lab container-review start`
- `sudo podman run -d -p 8080:80 --name httpd-basic redhattraining/httpd-parent:2.4`
- `sudo podman ps`
- `curl http://localhost:8080/`
- `sudo podman exec -it httpd-basic /bin/bash`
- `cd /var/www/html`
- `echo "Hello World" > index.html`
- `cat index.html`
- `exit`
- `curl http://localhost:8080/`
- `lab container-review grade`
- `lab container-review finish`
