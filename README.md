# IronHack-Lab8-API-Design

## API Design and Documentation Workshop:

#### Endpoint Design:
* Participants will define RESTful API endpoints for user management, order processing, and customer service interactions.

#### Resource Modeling:
* Define the data structures and resource models required to support the API functionality, such as user profiles, order details, and customer tickets.

#### Documentation:
* Document each API endpoint, specifying required parameters, possible responses, and error codes to ensure clarity and completeness. Emphasis will be placed on adherence to best practices in API documentation to enhance ease of use and maintainability.


## Solution

#### YAML

[E commerce.yaml](https://github.com/LiMonC/IronHack-Lab8-API-Design/blob/main/ecommerce_api.yaml)
        
```yaml
openapi: 3.0.0
info:
  title: Lab 8 - E-commerce System API
  description: APIS for an e-commerce system to managing user accounts, processing orders, and handling customer interactions efficiently.
  version: 1.0.0
servers:
  - url: https://api.ecommerce.com-dev/v1
  - url: https://api.ecommerce.com-qa/v1
  - url: https://api.ecommerce.com-prod/v1

components:
  securitySchemes:
    BearerAuth:
      type: http
      scheme: bearer

  schemas:
    User:
      type: object
      properties:
        Id:
          type: string
        name:
          type: string
        email:
          type: string
        password:
          type: string
        profile:
          type: string
    Order:
      type: object
      properties:
        Id:
          type: string
        products:
          type: array
          items:
            type: string
        total:
          type: number
          format: double
    Customer:
      type: object
      properties:
        Id:
          type: string
        name:
          type: string
        email:
          type: string
        phone:
          type: string
    Product:
      type: object
      properties:
        Id:
          type: string
        name:
          type: string
        price:
          type: string
    Error:
      type: object
      properties:
        code:
          type: integer
          format: int32
        message:
          type: string
        details:
          type: string

  responses:
    BadRequest:
      description: Invalid input
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
    Unauthorized:
      description: Unauthorized access
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
    NotFound:
      description: Resource not found
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
    InternalServerError:
      description: Internal server error
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'

security:
  - BearerAuth: []

paths:
  /users:
    get:
      summary: Get all users
      security:
        - BearerAuth: []
      responses:
        '200':
          description: Retrieve a list of users
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/User'
        '401':
          $ref: '#/components/responses/Unauthorized'
    post:
      summary: Create a new user
      security:
        - BearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/User'
      responses:
        '201':
          description: User created
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
  /users/{userId}:
    get:
      summary: Get a user detail by Id
      security:
        - BearerAuth: []
      parameters:
        - name: userId
          in: path
          required: true
          schema:
            type: string
      responses:
        '200':
          description: Get the user detail
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '404':
          $ref: '#/components/responses/NotFound'
        '401':
          $ref: '#/components/responses/Unauthorized'
    put:
      summary: Update a user by Id
      security:
        - BearerAuth: []
      parameters:
        - name: userId
          in: path
          required: true
          schema:
            type: string
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/User'
      responses:
        '200':
          description: User updated
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '400':
          $ref: '#/components/responses/BadRequest'
        '404':
          $ref: '#/components/responses/NotFound'
        '401':
          $ref: '#/components/responses/Unauthorized'
    delete:
      summary: Delete a user by Id
      security:
        - BearerAuth: []
      parameters:
        - name: userId
          in: path
          required: true
          schema:
            type: string
      responses:
        '204':
          description: User deleted
        '404':
          $ref: '#/components/responses/NotFound'
        '401':
          $ref: '#/components/responses/Unauthorized'

  /orders:
    get:
      summary: Get all orders
      security:
        - BearerAuth: []
      responses:
        '200':
          description: A list of orders
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Order'
        '401':
          $ref: '#/components/responses/Unauthorized'
    post:
      summary: Create a new order
      security:
        - BearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Order'
      responses:
        '201':
          description: Order created
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Order'
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
  /orders/{orderId}:
    get:
      summary: Get an order by Id
      security:
        - BearerAuth: []
      parameters:
        - name: orderId
          in: path
          required: true
          schema:
            type: string
      responses:
        '200':
          description: Order details
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Order'
        '404':
          $ref: '#/components/responses/NotFound'
        '401':
          $ref: '#/components/responses/Unauthorized'
    put:
      summary: Update an order by Id
      security:
        - BearerAuth: []
      parameters:
        - name: orderId
          in: path
          required: true
          schema:
            type: string
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Order'
      responses:
        '200':
          description: Order updated
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Order'
        '400':
          $ref: '#/components/responses/BadRequest'
        '404':
          $ref: '#/components/responses/NotFound'
        '401':
          $ref: '#/components/responses/Unauthorized'
    delete:
      summary: Delete an order by Id
      security:
        - BearerAuth: []
      parameters:
        - name: orderId
          in: path
          required: true
          schema:
            type: string
      responses:
        '204':
          description: Order deleted
        '404':
          $ref: '#/components/responses/NotFound'
        '401':
          $ref: '#/components/responses/Unauthorized'

  /customers:
    get:
      summary: Get all customers
      security:
        - BearerAuth: []
      responses:
        '200':
          description: A list of customers
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Customer'
        '401':
          $ref: '#/components/responses/Unauthorized'
    post:
      summary: Create a new customer
      security:
        - BearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Customer'
      responses:
        '201':
          description: Customer created
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Customer'
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
  /customers/{customerId}:
    get:
      summary: Get a customer by ID
      security:
        - BearerAuth: []
      parameters:
        - name: customerId
          in: path
          required: true
          schema:
            type: string
      responses:
        '200':
          description: Customer details
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Customer'
        '404':
          $ref: '#/components/responses/NotFound'
        '401':
          $ref: '#/components/responses/Unauthorized'
    put:
      summary: Update a customer by ID
      security:
        - BearerAuth: []
      parameters:
        - name: customerId
          in: path
          required: true
          schema:
            type: string
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Customer'
      responses:
        '200':
          description: Customer updated
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Customer'
        '400':
          $ref: '#/components/responses/BadRequest'
        '404':
          $ref: '#/components/responses/NotFound'
        '401':
          $ref: '#/components/responses/Unauthorized'
    delete:
      summary: Delete a customer by ID
      security:
        - BearerAuth: []
      parameters:
        - name: customerId
          in: path
          required: true
          schema:
            type: string
      responses:
        '204':
          description: Customer deleted
        '404':
          $ref: '#/components/responses/NotFound'
        '401':
          $ref: '#/components/responses/Unauthorized'
```



#### Reflection Report:
* A brief report where participants reflect on their design process, discussing:
        
* The challenges they faced during the API design.
* How they applied API First principles to ensure the API was robust and aligned with business needs.
* Insights gained from the exercise and potential improvements they could make in future API design projects.


Name and conventions to create robust api
        - Endpoint Naming 
        - Appropriate HTTP Method
        - API Requests and Responses Effectively
        - Appropriate Request Headers for Authentication
        - Actionable Error Messages

Designing and developing REST APIs that adhere to best practices is essential for creating robust, scalable, and secure software systems.
