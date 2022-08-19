# Tutorial for configure circle ci

1. Create an React App
2. Create NGINX folder to your app directory
3. Create nginx.conf file into NGINX folder
4. Write this below code inside your nginx.conf
```
server {
    listen 80;
    client_max_body_size 4G;

    keepalive_timeout 5;

    # path for static files
    root /usr/share/nginx/html;

    index index.html index.htm;

    location / {
        try_files $uri $uri/ /index.html =404;
        add_header   Cache-Control public;
        expires      1d;
    }

}
```
5. Download Docker to your computer
6. Sign up Dockerhub using this url https://hub.docker.com
7. Create file with name `Dockerfile` without file extentions to your working directory
8. Write this below code inside your Dockerfile
```
FROM node:14-alpine as build

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY ./ ./
RUN npm run build

FROM nginx
COPY --from=build /app/build /usr/share/nginx/html

COPY nginx/nginx.conf /etc/nginx/conf.d/default.conf

CMD [ "nginx", "-g", "daemon off;" ]
```
8. Build Docker Image
go to the terminal
and execute this commands
```
docker build -t <dockerhub_username>/<any_image_name>:<any_tag>
```
