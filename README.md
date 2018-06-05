# Insight DevOps Engineering Systems Puzzle

This repository contains Cheuk Lau's solution to the Insight DevOps coding challenge. The goal of the challenge is to find and resolve issues with a NGINX-Flask-Postgres application running in Docker. A summary of each issue and the steps taken to resolve them are provided below.

## Issue 1

### Problem

User is unable to connect to localhost:8080.

### Solution

In order for the user to connect to localhost:8080, port 8080 must be exposed from the NGINX container. This can be done with the following correction:

- `Dockerfile`: `EXPOSE 5001` should be changed to `EXPOSE 8080`

Next we need NGINX to map from port 8080 to 80. This can be done with the following correction:

- `docker-compose.yml`: `ports: - "8080:80"`

Finally, we need Flask to connect to NGINX through port 80. This can be done with the following corrections:

- `flaskapp.conf`: `proxy_pass http://flaskapp:5001` should be changed to `proxy_pass http://flaskapp:80`
- `app.py`: Add `port=80` to `app.run()`

