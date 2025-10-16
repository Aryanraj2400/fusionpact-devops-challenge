FROM nginx:alpine
WORKDIR /usr/share/nginx/html
COPY Devops_Intern.html index.html
RUN chmod -R 755 /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
