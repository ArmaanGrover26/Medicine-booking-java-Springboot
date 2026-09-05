# 💊 MediDelivery — Full-Stack Medicine Delivery Application

A modern, full-stack e-commerce and medicine delivery platform built with **Spring Boot 3**, **Spring Security (JWT)**, **MySQL**, and a responsive **React (Vite)** frontend. The application provides an end-to-end healthcare shopping experience, from browsing medicines and uploading prescriptions to order tracking and a dedicated administrative dashboard.

---

## 📑 Table of Contents

- [Features](#-features)
  - [Customer Experience](#customer-experience)
  - [Admin Management](#admin-management)
- [Tech Stack](#-tech-stack)
- [Project Architecture](#-project-architecture)
- [Database Schema](#-database-schema)
- [API Endpoints](#-api-endpoints)
- [Getting Started](#-getting-started)
  - [Prerequisites](#prerequisites)
  - [1. Database Configuration](#1-database-configuration)
  - [2. Backend Setup](#2-backend-setup)
  - [3. Frontend Setup](#3-frontend-setup)
- [Security & Authentication](#-security--authentication)
- [Folder Structure](#-folder-structure)
- [Future Enhancements](#-future-enhancements)
- [License](#-license)

---

## 🚀 Features

### Customer Experience
- **User Authentication**: Secure signup and login powered by Spring Security and JWT (JSON Web Tokens) with passwords hashed via BCrypt.
- **Product Catalog**: Browse and search medicines by name, category, health conditions, or popular products.
- **Product Detail**: Detailed medicine information including composition, dosage instructions, manufacturer, and pricing.
- **Cart & Order Checkout**: Add medicines to cart, adjust quantities, review order summary, and proceed through checkout.
- **Address Management**: Save multiple shipping addresses (Home, Work, etc.) tied to the authenticated user account.
- **Prescription Upload & Consultation**: Integrated prescription upload flow for prescription-only pharmaceuticals.
- **Health Articles & Blogs**: Informative healthcare blogs and articles with reading recommendations.
- **Interactive Health Chatbot**: Built-in interactive assistant widget to answer customer queries.
- **Order History**: Track past and active orders with real-time status updates (Processing, Shipped, Delivered).

### Admin Management
- **Admin Dashboard**: Overview metrics displaying total orders, users, and inventory status.
- **Order Management**: View all customer orders across the platform and update order delivery statuses.
- **Customer Control**: Manage registered customer accounts, view profiles, and block/unblock users.
- **Medicine Inventory**: Manage medicine listings, pricing, and stock details.

---

## 🛠️ Tech Stack

### Backend
- **Framework**: Spring Boot 3.1.5
- **Language**: Java 21
- **Security**: Spring Security 6 (Stateless JWT Authentication via `jjwt` 0.11.5)
- **Persistence**: Spring Data JPA, Hibernate ORM
- **Database**: MySQL 8.x
- **Build Tool**: Maven (Wrapper included)

### Frontend
- **Framework**: React 18
- **Bundler & Tooling**: Vite, ESLint
- **Routing**: React Router DOM (v6)
- **HTTP Client**: Axios (with custom request interceptor for JWT authorization)
- **Icons & Styling**: Lucide React, Modern Vanilla CSS with responsive design

---

## 🏗️ Project Architecture

```
┌────────────────────────────────────────────────────────┐
│               Frontend (React 18 + Vite)               │
│  - React Router (Pages & Views)                        │
│  - Context API (AuthContext, CartContext)              │
│  - Axios HTTP Client (Bearer JWT Interceptor)          │
└───────────────────────────▲────────────────────────────┘
                            │ REST API (JSON / HTTP)
                            │ Ports: 8080 <-> 5173 / 3000
┌───────────────────────────▼────────────────────────────┐
│              Backend (Spring Boot 3.1.5)               │
│  - Security Layer: JwtAuthenticationFilter + CORS      │
│  - Controllers: User, Order, Address, Admin            │
│  - Services: Business logic, Password hashing (BCrypt) │
│  - Repositories: Spring Data JPA Interfaces            │
└───────────────────────────▲────────────────────────────┘
                            │ JDBC / Hibernate
┌───────────────────────────▼────────────────────────────┐
│                 Database (MySQL 8.x)                   │
│  - Tables: users, addresses, orders, order_item        │
└────────────────────────────────────────────────────────┘
```

---

## 🗄️ Database Schema

The core relational database entities include:

- **`users`**: `id`, `full_name`, `email` (unique), `phone_number`, `password_hash`, `is_blocked`, `created_at`
- **`addresses`**: `id`, `user_id` (FK), `name`, `address`, `pincode`, `state`, `phone`, `type`
- **`orders`**: `id`, `user_id` (FK), `order_date`, `total_amount`, `status`, `shipping_name`, `shipping_address`, `shipping_phone`
- **`order_item`**: `id`, `order_id` (FK), `product_name`, `quantity`, `price`

---

## 📡 API Endpoints

### Authentication & Users (`/api/users`)
| Method | Endpoint | Access | Description |
| :--- | :--- | :--- | :--- |
| `POST` | `/api/users` | Public | Register a new user account |
| `POST` | `/api/users/login` | Public | Authenticate credentials and return JWT token |
| `GET` | `/api/users` | Authenticated | Retrieve all users |
| `GET` | `/api/users/{id}` | Authenticated | Retrieve user by ID |

### Addresses (`/api/addresses`)
| Method | Endpoint | Access | Description |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/addresses` | Authenticated | Get all saved addresses for the logged-in user |
| `POST` | `/api/addresses` | Authenticated | Add a new shipping address for the logged-in user |

### Orders (`/api/orders`)
| Method | Endpoint | Access | Description |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/orders` | Authenticated | Fetch order history for the logged-in user |
| `POST` | `/api/orders` | Authenticated | Create a new order with items and shipping details |

### Admin (`/api/admin`)
| Method | Endpoint | Access | Description |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/admin/users` | Authenticated | View all registered platform users |
| `PUT` | `/api/admin/users/{id}/status` | Authenticated | Block or unblock a user account |
| `GET` | `/api/admin/orders` | Authenticated | View all orders from all customers |
| `PUT` | `/api/admin/orders/{id}/status` | Authenticated | Update order fulfillment status (Processing, Shipped, Delivered) |

---

## 🏁 Getting Started

### Prerequisites
- **Java**: JDK 21 (or compatible 17+)
- **Node.js**: v18.0.0 or higher & `npm`
- **MySQL Server**: 8.0 or higher
- **Git**

---

### 1. Database Configuration

1. Start your local MySQL server.
2. Create the database:
   ```sql
   CREATE DATABASE Medicine;
   ```
3. Verify or update your database credentials in `Backend/src/main/resources/application.properties`:
   ```properties
   spring.datasource.url=jdbc:mysql://localhost:3306/Medicine?useSSL=false&allowPublicKeyRetrieval=true
   spring.datasource.username=root
   spring.datasource.password=YOUR_MYSQL_PASSWORD
   ```

---

### 2. Backend Setup

1. Open a terminal and navigate to the `Backend` directory:
   ```bash
   cd Backend
   ```
2. Build the application using the Maven Wrapper:
   - **Windows (PowerShell/CMD)**:
     ```powershell
     .\mvnw.cmd clean compile
     ```
   - **Linux / macOS**:
     ```bash
     ./mvnw clean compile
     ```
3. Run the Spring Boot server:
   - **Windows**:
     ```powershell
     .\mvnw.cmd spring-boot:run
     ```
   - **Linux / macOS**:
     ```bash
     ./mvnw spring-boot:run
     ```
4. The backend server will start on `http://localhost:8080`.

---

### 3. Frontend Setup

1. Open a new terminal and navigate to the `Frontend` directory:
   ```bash
   cd Frontend
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
3. Start the Vite development server:
   ```bash
   npm run dev
   ```
4. Open your browser and navigate to `http://localhost:5173`.

---

## 🔒 Security & Authentication

- **Stateless Sessions**: The backend is configured as `SessionCreationPolicy.STATELESS`. No server-side HTTP sessions are maintained.
- **JWT Provider**: On successful login, a cryptographically signed HMAC-SHA256 JWT token is issued.
- **Frontend Interceptor**: Axios interceptors automatically attach `Authorization: Bearer <token>` from `localStorage` to all API requests.
- **CORS Configured**: Cross-Origin Resource Sharing is enabled for `http://localhost:5173` and `http://localhost:3000`.

---

## 📁 Folder Structure

```text
Medicine-Delivery-App-SpringBoot/
├── Backend/
│   ├── src/main/java/com/MedicineDelivery/Backend/
│   │   ├── config/               # Security & CORS configuration
│   │   ├── controller/           # REST API controllers (User, Order, Address, Admin)
│   │   ├── dto/                  # Request/Response Data Transfer Objects
│   │   ├── model/                # JPA entities (User, Order, OrderItem, Address)
│   │   ├── repository/           # Spring Data JPA repositories
│   │   ├── security/             # JWT filter, UserDetailsService, Token provider
│   │   ├── service/              # Business logic services
│   │   └── BackendApplication.java
│   ├── src/main/resources/
│   │   └── application.properties# Database, JPA, and JWT settings
│   ├── pom.xml                   # Maven dependencies and build plugins
│   └── mvnw / mvnw.cmd           # Maven wrapper scripts
│
├── Frontend/
│   ├── public/                   # Static assets & favicon
│   ├── src/
│   │   ├── assets/               # Product images, logos, illustrations
│   │   ├── components/           # Reusable UI components & Admin widgets
│   │   ├── context/              # React Contexts (AuthContext, CartContext)
│   │   ├── pages/                # Main view pages (Home, Products, Cart, Checkout, Admin)
│   │   ├── App.jsx               # Route definitions
│   │   ├── main.jsx              # React DOM root mounting
│   │   └── productData.js        # Catalog mock data & metadata
│   ├── package.json              # Frontend npm dependencies
│   ├── vite.config.js            # Vite build configuration
│   └── .gitignore                # Node modules and build output exclusions
│
├── .gitignore                    # Root gitignore
└── README.md                     # Project documentation
```

---

## 🔮 Future Enhancements

- [ ] Integrate online payment gateways (Stripe / Razorpay).
- [ ] Implement email/SMS notifications for order status changes.
- [ ] Add prescription approval workflow in the Admin dashboard.
- [ ] Implement Refresh Tokens for seamless session renewal.
- [ ] Role-based endpoint guards (`ROLE_ADMIN` vs `ROLE_USER`) using Spring Security method security.

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).
