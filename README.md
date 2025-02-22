#install docker
dnf -y upadte
sudo dnf remove -y docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-selinux docker-engine-selinux docker-engine
sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo dnf install docker-ce docker-ce-cli containerd.io --nobest -y
sudo systemctl start docker
sudo systemctl enable docker
#create Dockerfile directory
mkdir nginx-docker
cd nginx-docker
touch Dockerfile
nano Dockerfile
FROM nginx:latest  
EXPOSE 8080  
RUN rm /etc/nginx/conf.d/default.conf  
COPY nginx.conf /etc/nginx/conf.d/default.conf  
COPY ./html /usr/share/nginx/html
#create nginx file
touch nginx.conf
nano nginx.conf
server {
    listen 8080;
    server_name localhost;

    location / {
        root /usr/share/nginx/html;
        index index.html;
                allow 1.1.1.1;
        deny all;
    }
}
#create index.html
mkdir html
touch html/index.html
nano html/index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Dockerized Nginx</title>
</head>
<body>
    <h1>Welcome to my Nginx Docker container!</h1>
</body>
</html>

docker build -t my-nginx .
docker run -d --restart always --name my-nginx-container -p 8080:8080 my-nginx

