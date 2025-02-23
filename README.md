Steps to Set Up the Project

1. Install Docker

Ensure that Docker is installed on your Linux system by following the official guide. Verify the installation with:

`sudo docker --version`

If Docker is not installed, you can install it using:

```
curl -fsSL https://get.docker.com | sh
sudo systemctl start docker
sudo systemctl enable docker
```

2. Create a Dockerfile

Create a file named `Dockerfile` with the following content:

```
FROM nginx:latest
RUN rm /etc/nginx/conf.d/default.conf
COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY ./html /usr/share/nginx/html
```

This file:

1. Pulls the latest Nginx image.

2. Removes the default configuration.

3. Copies a custom Nginx configuration file.

4. Copies the website files into the Nginx root directory.

3. Configure Nginx

Create a file named `nginx.conf` with the following content:

```
server {
    listen 8080;
    server_name localhost;

    location / {
        root /usr/share/nginx/html;
        index index.html;
        allow YOUR_IP_HERE;
        deny all;
    }
}
```

Replace `YOUR_IP_HERE` with the specific IP address allowed to access the website.

4. Create Website Content

Inside the project directory, create an html folder and add an `index.html` file:

```
<!DOCTYPE html>
<html>
<head>
    <title>Welcome</title>
</head>
<body>
    <h1>Welcome to my website</h1>
    <p>This site is set up as part of a task for the company Mohaymen.</p>
    <p>Thank you for visiting!</p>
</body>
</html>
```

5. Build the Docker Image

Run the following command to build the Docker image:

`docker build -t my-nginx .`

6. Run the Container

Run the Nginx container on port 8080 with automatic restart:

`docker run -d --restart always --name my-nginx-container -p 8080:8080 my-nginx`

7. Verify and Test

Check if the container is running:

`docker ps`

Access the website in your browser:

`http://YOUR_SERVER_IP:8080`

8. Stop and Remove the Container (if needed)

If you need to stop and remove the container:

```
docker stop my-nginx-container

docker rm my-nginx-container
```

Conclusion

This project successfully deploys an Nginx web server inside a Docker container with restricted IP access. The website is hosted on port 8080 and will automatically restart if the container stops.
