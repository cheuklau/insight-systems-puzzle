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

## Issue 2

### Problem

The list of items in the repository does not properly show in localhost:8080/success.

### Solution

The Flask `/success` route originally returned the query result cast as a string:

- `return str(results)`

This is incorrect as we want to display the query result as HTML for the user to see. This can be done with the following correction:

- Replace `return str(results)` with `return render_template('showitems.html', results=results)`

The above change passes the query result into a template called `showitems.html` which loops through each item in the query and displays its `name`, `quantity` and `description` using HTML markup.

## Additional comments

A better design of this website would be to show the list of current items in the homepage with a link to add an item. The page to add an item page should allow the user to cancel (which would return the user back to the homepage) or confirm which would add the new item to the database then return the user back to the homepage with a flash message notifying the user that the item has been added to the database. 