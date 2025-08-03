# MiniCommerce - Laravel E-commerce Application

A comprehensive e-commerce web application built with Laravel, featuring advanced database functionality including SQL Views, Stored Procedures, and Triggers.

## Features

### Core Functionality

-   **User Authentication** - Registration, login, logout using Laravel Breeze
-   **Role-based Access Control** - Admin and Customer roles
-   **Product Management** - CRUD operations for products with categories
-   **Shopping Cart System** - Add, remove, update cart items
-   **Order Management** - Place orders with Cash on Delivery simulation
-   **Category Management** - Organize products by categories

### Advanced Database Features

-   **SQL View**: `view_order_summary` - Complete order information with user and product details
-   **Stored Procedure**: `update_stock(product_id, qty_sold)` - Safe stock updates after orders
-   **Database Trigger**: Automatic audit logging for product updates

### Technology Stack

-   **Backend**: Laravel 12.x, PHP 8.2+
-   **Frontend**: Blade Templates, Tailwind CSS
-   **Database**: MySQL (via XAMPP)
-   **Authentication**: Laravel Breeze
-   **File Storage**: Local storage with symbolic linking

## Installation & Setup

### Prerequisites

-   XAMPP (MySQL, Apache, PHP 8.2+)
-   Composer
-   Node.js & NPM

### Installation Steps

1. **Clone the repository**

    ```bash
    git clone <repository-url>
    cd ecommerce
    ```

2. **Install PHP dependencies**

    ```bash
    composer install
    ```

3. **Install Node dependencies**

    ```bash
    npm install
    ```

4. **Environment Configuration**

    ```bash
    cp .env.example .env
    ```

    Update `.env` file:

    ```
    APP_NAME=MiniCommerce
    DB_CONNECTION=mysql
    DB_HOST=127.0.0.1
    DB_PORT=3306
    DB_DATABASE=minicommerce
    DB_USERNAME=root
    DB_PASSWORD=
    ```

5. **Generate Application Key**

    ```bash
    php artisan key:generate
    ```

6. **Create Storage Link**

    ```bash
    php artisan storage:link
    ```

7. **Run Migrations**

    ```bash
    php artisan migrate
    ```

8. **Seed Database**

    ```bash
    php artisan db:seed
    ```

9. **Build Frontend Assets**

    ```bash
    npm run build
    ```

10. **Start Development Server**
    ```bash
    php artisan serve
    ```

The application will be available at `http://localhost:8000`

## Default Credentials

### Admin Account

-   **Email**: admin@minicommerce.com
-   **Password**: password

### Customer Accounts

-   **Email**: john@example.com / **Password**: password
-   **Email**: jane@example.com / **Password**: password
-   **Email**: bob@example.com / **Password**: password

## Database Structure

### Tables

-   `users` - User accounts with role column    
-   `categories` - Product categories
-   `products` - Product catalog with inventory
-   `cart_items` - Shopping cart contents
-   `orders` - Customer orders
-   `order_items` - Individual items within orders
-   `product_audit_log` - Audit trail for product changes

### Advanced Database Features

#### SQL View: view_order_summary

```sql
SELECT
    o.id as order_id,
    o.order_number,
    o.created_at as order_date,
    o.status,
    o.payment_status,
    o.total_amount,
    u.name as customer_name,
    u.email as customer_email,
    oi.product_id,
    p.name as product_name,
    oi.quantity,
    oi.price as unit_price,
    oi.total as item_total
FROM orders o
JOIN users u ON o.user_id = u.id
JOIN order_items oi ON o.id = oi.order_id
JOIN products p ON oi.product_id = p.id
```

#### Stored Procedure: update_stock

```sql
CALL update_stock(product_id, quantity_sold);
```

#### Trigger: product_update_audit

Automatically logs changes to products in the `product_audit_log` table.

## Application Routes

### Public Routes

-   `/` - Home page
-   `/products` - Product catalog
-   `/products/{product}` - Product details
-   `/categories/{category}` - Products by category
-   `/login` - User login
-   `/register` - User registration

### Authenticated User Routes

-   `/cart` - Shopping cart
-   `/checkout` - Checkout process
-   `/orders` - Order history
-   `/profile` - User profile management

### Admin Routes (Admin role required)

-   `/admin/dashboard` - Admin dashboard with statistics
-   `/admin/categories` - Category management
-   `/admin/products` - Product management
-   `/admin/orders` - Order management

## Key Features

### For Customers

-   Browse products by category
-   Search and filter products
-   Add items to shopping cart
-   Place orders with shipping information
-   View order history and status
-   Cash on Delivery payment simulation

### For Administrators

-   Dashboard with business metrics
-   Complete product and category management
-   Order monitoring and status updates
-   Inventory tracking with low stock alerts
-   Product audit trail viewing
-   User management capabilities

## File Structure

```
app/
├── Http/Controllers/
│   ├── Admin/          # Admin panel controllers
│   ├── CartController.php
│   ├── HomeController.php
│   ├── OrderController.php
│   └── ProductController.php
├── Models/             # Eloquent models with relationships
├── Policies/           # Authorization policies
└── Http/Middleware/    # Custom middleware

database/
├── migrations/         # Database schema
└── seeders/           # Sample data

resources/
├── views/             # Blade templates
└── css/js/           # Frontend assets
```

## Security Features

-   CSRF protection on all forms
-   SQL injection prevention through Eloquent ORM
-   Authorization policies for cart and order access
-   Role-based access control
-   Input validation and sanitization

## Advanced Database Examples

### Using the SQL View

```php
$orderSummary = DB::select('SELECT * FROM view_order_summary WHERE order_id = ?', [$orderId]);
```

### Calling the Stored Procedure

```php
DB::statement('CALL update_stock(?, ?)', [$productId, $quantity]);
```

### Viewing Audit Logs

```php
$auditLogs = ProductAuditLog::with('user', 'product')
    ->where('product_id', $productId)
    ->latest()
    ->get();
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## License

This project is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).

## Support

For support and questions, please create an issue in the repository or contact the development team.

---

**MiniCommerce** - A modern, feature-rich e-commerce solution built with Laravel and advanced database features.
