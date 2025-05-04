# airbnb-clone-project

## Team Roles
### 1. Backend Developer
- **Description**: Focuses on server-side logic, APIs, and integration with databases/services.
- **Responsibilities**:
  - Develops and maintains the core application logic.
  - Implements security and data protection measures.
  - Optimizes performance and scalability.
 
### 2. Technology Stack
- **Technologies**: Django, PostgreSQL, GraphQL
- **Django**  
  A high-level Python web framework used to build robust RESTful APIs and handle server-side logic, authentication, and routing.  
  *Why?* Provides scalability, security (e.g., CSRF protection), and a clean MVC architecture.

- **PostgreSQL**  
  A relational database system chosen for its reliability, ACID compliance, and advanced features (e.g., JSON support).  
  *Why?* Handles complex queries and ensures data integrity for user accounts, transactions, etc.

- **GraphQL**  
  A query language for APIs that enables efficient data fetching (clients request only what they need).  
  *Why?* Reduces over-fetching and under-fetching of data compared to REST, improving frontend performance.

## Database Design

The database schema consists of the following entities and relationships:

### 1. **User**
- **Fields**:  
  `id` (UUID/PK), `email` (unique), `password_hash`, `name`, `role` (host/guest)  
- **Relationships**:  
  - One-to-Many with `Property` (A user can list multiple properties).  
  - One-to-Many with `Booking` (A user can make multiple bookings).  
  - One-to-Many with `Review` (A user can write multiple reviews).  

### 2. **Property**
- **Fields**:  
  `id` (UUID/PK), `title`, `description`, `price_per_night`, `location`, `host_id` (FK to User)  
- **Relationships**:  
  - Many-to-One with `User` (Each property belongs to one host).  
  - One-to-Many with `Booking` (A property can have multiple bookings).  
  - One-to-Many with `Review` (A property can receive multiple reviews).  

### 3. **Booking**
- **Fields**:  
  `id` (UUID/PK), `start_date`, `end_date`, `total_price`, `status` (confirmed/pending/cancelled), `guest_id` (FK to User), `property_id` (FK to Property)  
- **Relationships**:  
  - Many-to-One with `User` (A booking is made by one guest).  
  - Many-to-One with `Property` (A booking is for one property).  
  - One-to-One with `Payment` (Each booking has one payment).  

### 4. **Review**
- **Fields**:  
  `id` (UUID/PK), `rating` (1-5), `comment`, `created_at`, `author_id` (FK to User), `property_id` (FK to Property)  
- **Relationships**:  
  - Many-to-One with `User` (A review is written by one user).  
  - Many-to-One with `Property` (A review is about one property).  

### 5. **Payment**
- **Fields**:  
  `id` (UUID/PK), `amount`, `payment_method`, `status` (success/failed), `transaction_id`, `booking_id` (FK to Booking)  
- **Relationships**:  
  - One-to-One with `Booking` (A payment corresponds to one booking).  

---

### Schema Diagram (Conceptual)
```plaintext
User ────┐
  │      │
  ├─ Property ──── Booking ─── Payment
  │        │
  └── Review
