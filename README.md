# Pizza Delivery API

This is a REST API for a Pizza Delivery service, built using FastAPI, SQLAlchemy, and PostgreSQL.

## Routes Implemented

The API provides the following routes:

| METHOD | ROUTE | FUNCTIONALITY | ACCESS |
|---|---|---|---|
| `POST` | `/auth/signup/` | Register new user | All users |
| `POST` | `/auth/login/` | _Login user_ | All users |
| `POST` | `/orders/order/` | _Place an order_ | All users |
| `PUT` | `/orders/order/update/{order_id}/` | _Update an order_ | All users |
| `PATCH` | `/orders/order/status/{order_id}/` | _Update order status_ | _Superuser_ |
| `DELETE` | `/orders/order/delete/{order_id}/` | _Delete/Remove an order_ | All users |
| `GET` | `/orders/user/orders/` | _Get user's orders_ | All users |
| `GET` | `/orders/orders/` | _List all orders made_ | _Superuser_ |
| `GET` | `/orders/orders/{order_id}/` | _Retrieve an order_ | _Superuser_ |
| `GET` | `/orders/user/order/{order_id}/` | _Get user's specific order_ | All users |
| `GET` | `/docs/` | _View API documentation_ | All users |

## How to Run the Project

To run this project, follow these steps:

1.  **Install PostgreSQL**: Ensure PostgreSQL is installed on your system.
2.  **Install Python**: Make sure Python is installed.
3.  **Clone the Repository**:
    ```bash
    git clone [https://github.com/Swarookali-3121/PIZZA-DELIVERY-API.git)
    ```
4.  **Create and Activate Virtual Environment**:
    Navigate to the project directory and create a virtual environment using `Pipenv` or `virtualenv`.
    ```bash
    # Using virtualenv
    python -m venv env
    source env/bin/activate # On Windows use `env\Scripts\activate`
    ```
5.  **Install Requirements**:
    ```bash
    pip install -r requirements.txt
    ```
6.  **Set Up PostgreSQL Database URI**:
    Open `database.py` and update the `create_engine` line with your PostgreSQL username, password, and database name.
    ```python
    engine=create_engine('postgresql://postgres:<username>:<password>@localhost/<db_name>',
        echo=True
    )
    ```
    For example, it might look like this:
    ```python
    engine=create_engine('postgresql://postgres:swaroop@localhost:5432/pizza_delivery',
        echo=True
    )
    ```
7.  **Create Database Tables**:
    ```bash
    python init_db.py
    ```
8.  **Run the API**:
    ```bash
    uvicorn main:app
    ```

## Project Structure and Explanation

* **`main.py`**: This is the main FastAPI application file. It includes the `auth_router` and `order_router` and sets up JWT authentication. It also customizes the OpenAPI documentation to include security schemes.
* **`auth_routes.py`**: This file defines authentication-related API endpoints.
    * `POST /auth/signup`: Registers a new user.
    * `POST /auth/login`: Logs in a user, generating access and refresh tokens.
    * `GET /auth/refresh`: Refreshes an access token using a refresh token.
* **`order_routes.py`**: This file contains API endpoints related to order management.
    * `POST /orders/order`: Allows any authenticated user to place a new order.
    * `GET /orders/orders`: Lists all orders, accessible only by superusers.
    * `GET /orders/orders/{id}`: Retrieves a specific order by ID, accessible only by superusers.
    * `GET /orders/user/orders`: Lists all orders made by the currently logged-in user.
    * `GET /orders/user/order/{id}/`: Retrieves a specific order by ID for the currently logged-in user.
    * `PUT /orders/order/update/{id}/`: Updates an existing order.
    * `PATCH /orders/order/status/{id}/`: Updates the status of an order, accessible only by superusers.
    * `DELETE /orders/order/delete/{id}/`: Deletes an order.
* **`models.py`**: This file defines the SQLAlchemy ORM models for the database tables:
    * `User`: Represents a user with fields like `username`, `email`, `password`, `is_staff`, and `is_active`. It has a one-to-many relationship with `Order`.
    * `Order`: Represents an order with fields like `quantity`, `order_status`, `pizza_size`, and `user_id`.
* **`schemas.py`**: This file defines Pydantic models used for data validation and serialization of API requests and responses.
    * `SignUpModel`: Schema for user registration.
    * `LoginModel`: Schema for user login.
    * `OrderModel`: Schema for placing and updating orders.
    * `OrderStatusModel`: Schema for updating the status of an order.
    * `Settings`: Used for configuring `fastapi_jwt_auth` with a secret key.
* **`database.py`**: Configures the SQLAlchemy engine and session for connecting to the PostgreSQL database.
* **`init_db.py`**: A script to create all defined database tables based on the SQLAlchemy models.
* **`requirements.txt`**: Lists all Python dependencies required for the project.
* **`.gitignore`**: Specifies files and directories to be ignored by Git (e.g., `__pycache__`, `env`).
