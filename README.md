# Goals App
This is a basic multilayer app to be dockerized

---
## How to run

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

5. Move to the frontend application
    ```console
    cd frontend/
    ```

6. Install dependencies
    ```console
    npm install
    ```

7. Start frontend application
    ```console
    npm start
    ```

8. Now, you can visit your __localhost:3000__
![goals_frontend](assets/goals_frontend.png)
