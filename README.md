# DeshiDrip — E-commerce Clothing Store

> An online clothing brand blending modern fashion with *deshi* culture.
> A full-stack PHP & MySQL web store with a customer storefront and an admin panel.

**Team project.** Originally built with [@joha786](https://github.com/joha786) and teammates. Original repository: [joha786/DeshiDrip-Clothing](https://github.com/joha786/DeshiDrip-Clothing).

![PHP](https://img.shields.io/badge/PHP-777BB4?logo=php&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?logo=mysql&logoColor=white)
![Architecture](https://img.shields.io/badge/Architecture-MVC-informational)

---

## My contribution

I was responsible for:

- **Database design.** I designed the relational MySQL schema (`database/clothing_brand.sql`): 7 linked tables covering users, a hierarchical category tree, products with sizes and stock, cart, orders, order items and payments, plus sample data for testing.
- **Cart and checkout.** I built the cart and order flow: adding items with a selected size, checkout, creating the order and its line items, recording payments, and reducing product stock when an order is placed.

---

## Features

**Customers**
- Browse the catalogue by Men / Women and category (shirts, pants, T-shirts, salwar, jeans, kurtis)
- Search products and view details with size charts and available sizes
- Add to cart with a chosen size, then check out
- Register and log in, with a "remember me" option
- View profile and purchase history, and cancel pending orders

**Admin**
- Dashboard showing total products, customers, orders and pending orders
- Manage products (featured and new-arrival flags, stock, images)
- Review orders and change their status (pending → confirmed / rejected / cancelled)
- View customers and order history

---

## Tech stack

| Layer | Technology |
|---|---|
| Backend | PHP (MVC: `model/`, `view/`, `controller/`) |
| Database | MySQL / MariaDB via `mysqli` |
| Frontend | HTML, CSS, JavaScript (AJAX handlers in `ajax/`) |
| Auth | PHP sessions, hashed passwords, remember-me token, role-based access (admin / customer) |

---

## Database schema

```
users ──< orders ──< order_items >── products >── categories (self-referencing parent_id)
  │          │
  │          └──< payments
  └──< cart >── products
```

| Table | Purpose |
|---|---|
| `users` | Customers and admins (role, contact details, password hash) |
| `categories` | Category tree (Men → Shirts, Women → Kurtis, …) |
| `products` | Name, price, sizes, stock, gender, featured / new-arrival flags |
| `cart` | Items a user has added, with selected size |
| `orders` | One row per order: total, status, date |
| `order_items` | Line items: product, quantity, size, unit price |
| `payments` | Payment method, amount and transaction ID per order |

Example query (orders with customer details):

```sql
SELECT orders.*, users.name AS customer_name, users.email
FROM orders
INNER JOIN users ON orders.user_id = users.id
ORDER BY orders.order_date DESC;
```

---

## Project structure

```
DeshiDrip-Clothing/
├── ajax/         # AJAX handlers: cart, checkout, order status, cancel order, product search
├── config/       # db.php (connection) and auth.php (sessions, roles)
├── controller/   # AuthController, ProductController
├── database/     # clothing_brand.sql (schema and sample data)
├── model/        # CartModel, OrderModel, ProductModel, UserModel
├── public/       # CSS, JS, product images
├── view/         # Storefront and admin pages
└── index.php     # Redirects to view/home.php
```

---

## Running it locally

1. Install [XAMPP](https://www.apachefriends.org/) (Apache + MySQL + PHP).
2. Clone this repository into `htdocs`:
   ```bash
   git clone https://github.com/dipjee678/DeshiDrip-Clothing.git
   ```
3. Start **Apache** and **MySQL** from the XAMPP control panel.
4. Open **phpMyAdmin** and import `database/clothing_brand.sql`. This creates the `clothing_brand` database with sample categories and products.
5. If your MySQL user or password is not `root` with an empty password, update `config/db.php`.
6. Visit `http://localhost/DeshiDrip-Clothing/`.

Sample admin and customer accounts are included in the SQL file for testing.

---

## Credits

Built as a team project. Original repository by [@joha786](https://github.com/joha786): [joha786/DeshiDrip-Clothing](https://github.com/joha786/DeshiDrip-Clothing).
