Sprint 1: System Architecture & Scope Definition
Project: FreshCart — Online Grocery Store
Section 1: Target Audience & Market Focus
	•	Primary Persona: Urban and suburban households and working professionals who want to order daily groceries and food essentials online instead of visiting physical stores.
	•	Core Pain Point: Buyers waste time commuting to local markets, deal with limited stock/variety, and have no easy way to track orders or reorder frequently-used items.
	•	Domain Scope: Grocery & Food Services (packaged foods, fresh produce, dairy, household essentials).
  Section 2: MVP Feature Scope
  |Category      |Feature Name                      |Description                                                                       |Priority  |
|--------------|----------------------------------|----------------------------------------------------------------------------------|----------|
|Authentication|User Registration & Authentication|Signup/login with password hashing and JWT-based session authentication.          |High (MVP)|
|Catalog       |Product List & Search             |Browse grocery items with category-based filtering (e.g., dairy, snacks, produce).|High (MVP)|
|Cart          |Cart Management                   |Persistent cart — add, update quantity, remove items.                             |High (MVP)|
|Checkout      |Order Processing                  |Mock/Stripe payment integration, generates an order record.                       |High (MVP)|
|Admin         |Inventory Control                 |Admin CRUD for products and stock quantity updates.                               |Medium    |
|Orders        |Order History & Tracking          |User can view past orders and current order status.                               |Medium    |

Section 3: Tech Stack Selection & Justification
	•	Frontend Framework: React
Justification: Component-based structure suits a catalog + cart UI well, has a huge ecosystem, and is easy for a student team to learn and debug quickly within a semester.
	•	Backend Infrastructure: Node.js / Express
Justification: Express is lightweight and fast to set up for REST APIs, pairs naturally with a JS frontend (single language across the stack), and has strong community support for auth (JWT) and payment integrations.
	•	Database Management System: MySQL
Justification: Grocery data (users, products, orders, order items) is highly relational with clear foreign-key relationships and needs strong data integrity (e.g., stock counts, order totals) — a relational DB fits better than a NoSQL store here.
	•	Caching & Asynchronous Processing (Optional): Redis
Justification: Can be used to cache frequently-viewed product listings and store session/cart data temporarily for faster response times.

Section 4: Entity-Relationship Diagram (ERD)
erDiagram
    USERS ||--o{ ORDERS : places
    USERS ||--o{ CART_ITEMS : has
    ORDERS ||--|{ ORDER_ITEMS : contains
    PRODUCTS ||--o{ ORDER_ITEMS : ordered_in
    PRODUCTS ||--o{ CART_ITEMS : added_to
    CATEGORIES ||--o{ PRODUCTS : categorizes

    USERS {
        int id PK
        string email
        string password_hash
        string full_name
        timestamp created_at
    }

    CATEGORIES {
        int id PK
        string name
        string description
    }

    PRODUCTS {
        int id PK
        int category_id FK
        string name
        decimal price
        int stock_quantity
        string unit
    }

    CART_ITEMS {
        int id PK
        int user_id FK
        int product_id FK
        int quantity
    }

    ORDERS {
        int id PK
        int user_id FK
        decimal total_amount
        string status
        timestamp created_at
    }

    ORDER_ITEMS {
        int id PK
        int order_id FK
        int product_id FK
        int quantity
        decimal unit_price
    }

Relationship & Key Notes
	•	USERS → ORDERS: 1:N — one user can place many orders (ORDERS.user_id FK → USERS.id).
	•	USERS → CART_ITEMS: 1:N — one user has many items in their cart.
	•	ORDERS → ORDER_ITEMS: 1:N — one order contains many line items (ORDER_ITEMS.order_id FK → ORDERS.id).
	•	PRODUCTS → ORDER_ITEMS: 1:N — a product can appear in many order items (ORDER_ITEMS.product_id FK → PRODUCTS.id), together forming an N:M relationship between Orders and Products.
	•	PRODUCTS → CART_ITEMS: 1:N — same pattern as above, resolving the N:M between Users' carts and Products.
	•	CATEGORIES → PRODUCTS: 1:N — one category has many products (PRODUCTS.category_id FK → CATEGORIES.id).
  
