# Goals App
Basic web application to manage your goals.

Application components:
- Database: Mongo database to store the data.
- Backend: A node and express application that expose endpoints to create goals and connect with the database.
- Frontend: A react application that exposes a simple UI on the web browser.

# How to run

## __Using Docker compose__

Deploy all the components in one single command using `docker-compose`

1. Starting the services
    ```console
    docker-compose up --build
    ```
    :memo: Use the `--build` flag to build the images from the dockerfiles before starting containers. You can omit it if you have already built the images.

2. Now, you can visit your __localhost:3000__

    ![goals_frontend](assets/goals_frontend.png)


:stop_sign: Run the following command to stop the services
```console
docker-compose down -v
```

## __Using Docker__

Configure and deploy each component using `docker`

1. Create and run a container with the mongo db
    ```console
    docker run -d --name mongo-db --rm mongo
    ```

2. Build the image for the backend application
    ```console
    docker build -t goals-js-backend ./backend
    ```

3. Look for the db IP address
    ```console
    docker inspect mongo-db | grep -e IPAddress
    ```
    you should get an output similar to the following:
    
    ![container_ip_address](assets/container_ip_address.png)
    
    Copy the __IPAddress__

4. Run the backend application

    Use the container __IPAddress__ copied in the previous step instead of `{mongo-db-address}` in the following command:

    ```console
    docker run -d -p 80:80 -e APP_PORT=80 -e MONGO_DATASOURCE_URL=mongodb://{mongo-db-address}:27017/course-goals --name goals-backend --rm goals-js-backend
    ```

5. Build the image for frontend
    ```console
    docker build -t goals-js-frontend ./frontend
    ```

6. Run the frontend 
    ```console
    docker run -d -p 3000:3000 --rm --name goals-frontend goals-js-frontend
    ``` 

7. Now, you can visit your __localhost:3000__

    ![goals_frontend](assets/goals_frontend.png)
    
:stop_sign: Run the following command to stop the services
```console
docker stop mongo-db goals-backend goals-frontend 
```

# Notes

:warning: Note that the react application for the frontend run on the browser, so in this case localhost refers to the host machine where the browser is running instead of the application container.

:triangular_flag_on_post: As mentioned in the previous point the application runs in the browser and tries to communicate with the backend on the browser's local host. In this case if you try to access from another machine you will get an error because the backend is on the localhost where the container is running.

![goals_frontend_outside_access](assets/goals_frontend_outside_access.png)
