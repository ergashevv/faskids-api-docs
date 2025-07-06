# FAS KIDS API Documentation

## Table of Contents
1. [Overview](#overview)
2. [Authentication](#authentication)
3. [Base URL](#base-url)
4. [Endpoints](#endpoints)
5. [Filters & Query Parameters](#filters--query-parameters)
6. [Response Formats](#response-formats)
7. [Error Handling](#error-handling)
8. [Frontend Integration](#frontend-integration)
9. [Examples](#examples)

## Overview

FAS KIDS is a full-stack e-commerce platform for children's clothing. This API provides comprehensive endpoints for managing products, orders, users, categories, and more.

**Environment URLs:**
- **Development:** `http://localhost:5001/api`
- **Production:** `https://api.faskids.shop/api`
- **Swagger Docs:** `http://localhost:5001/api-docs` (Dev) / `https://api.faskids.shop/api-docs` (Prod)

## Authentication

### JWT Token Authentication

Most endpoints require authentication using JWT Bearer tokens.

**Login to get token:**
```bash
POST /api/auth/login
Content-Type: application/json

{
  "phone": "+998901234567",
  "password": "your_password"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Login successful",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "_id": "507f1f77bcf86cd799439011",
    "fullName": "John Doe",
    "phone": "+998901234567",
    "role": "user"
  }
}
```

**Using token in requests:**
```bash
GET /api/users/profile
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

### User Roles

- **user:** Regular customer
- **admin:** Store administrator
- **superadmin:** System super administrator
- **cashier:** Order processing staff
- **courier:** Delivery personnel

## Base URL

```javascript
// Development
const API_BASE_URL = 'http://localhost:5001/api';

// Production
const API_BASE_URL = 'https://api.faskids.shop/api';
```

## Endpoints

### Authentication Endpoints

#### 1. User Registration
```bash
POST /api/auth/register
Content-Type: application/json

{
  "firstName": "John",
  "lastName": "Doe",
  "phone": "+998901234567",
  "password": "password123",
  "children": [
    {
      "name": "Alice",
      "age": 5,
      "gender": "female"
    }
  ]
}
```

**Response:**
```json
{
  "success": true,
  "message": "User registered successfully",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "_id": "507f1f77bcf86cd799439011",
    "fullName": "John Doe",
    "phone": "+998901234567",
    "role": "user"
  }
}
```

#### 2. User Login
```bash
POST /api/auth/login
Content-Type: application/json

{
  "phone": "+998901234567",
  "password": "password123"
}
```

#### 3. Logout
```bash
POST /api/auth/logout
Authorization: Bearer <token>
```

#### 4. Get Profile
```bash
GET /api/users/profile
Authorization: Bearer <token>
```

#### 5. Update Profile
```bash
PUT /api/users/profile
Authorization: Bearer <token>
Content-Type: application/json

{
  "fullName": "John Updated",
  "phone": "+998901234567",
  "gender": "male",
  "dateOfBirth": "1990-01-01",
  "address": {
    "street": "123 Main St",
    "city": "Tashkent",
    "state": "Tashkent",
    "zipCode": "100000"
  },
  "preferences": {
    "language": "uz",
    "currency": "UZS"
  }
}
```

### Product Endpoints

#### 1. Get All Products
```bash
GET /api/products?category=507f1f77bcf86cd799439011&gender=boy&ageRange=5-8&minPrice=100000&maxPrice=500000&sortBy=price&sortOrder=asc&page=1&limit=20
```

**Response:**
```json
{
  "success": true,
  "data": {
    "products": [
      {
        "_id": "507f1f77bcf86cd799439011",
        "title": {
          "uz": "O'g'il bolalar ko'ylagi",
          "en": "Boys T-Shirt",
          "ru": "Футболка для мальчиков"
        },
        "description": {
          "uz": "Yaxshi sifatli mato",
          "en": "High quality fabric",
          "ru": "Качественная ткань"
        },
        "price": 150000,
        "quantity": 50,
        "category": "507f1f77bcf86cd799439012",
        "gender": "boy",
        "ageRange": "5-8",
        "sizes": ["S", "M", "L"],
        "images": ["/uploads/product1.jpg"],
        "status": "active",
        "createdAt": "2024-01-01T00:00:00.000Z"
      }
    ],
    "pagination": {
      "currentPage": 1,
      "totalPages": 5,
      "totalItems": 100,
      "itemsPerPage": 20,
      "hasNextPage": true,
      "hasPrevPage": false
    }
  }
}
```

#### 2. Get Single Product
```bash
GET /api/products/507f1f77bcf86cd799439011
```

#### 3. Search Products
```bash
GET /api/products/search?q=ko'ylak&category=507f1f77bcf86cd799439011
```

#### 4. Get Related Products
```bash
GET /api/products/507f1f77bcf86cd799439011/related
```

#### 5. Create Product (Admin)
```bash
POST /api/products
Authorization: Bearer <admin_token>
Content-Type: multipart/form-data

{
  "title": {
    "uz": "Yangi ko'ylak",
    "en": "New T-Shirt",
    "ru": "Новая футболка"
  },
  "description": {
    "uz": "Tavsif",
    "en": "Description",
    "ru": "Описание"
  },
  "price": 150000,
  "quantity": 50,
  "category": "507f1f77bcf86cd799439012",
  "gender": "boy",
  "ageRange": "5-8",
  "sizes": ["S", "M", "L"],
  "status": "active",
  "season": "summer",
  "images": [File1, File2, File3]
}
```

#### 6. Update Product (Admin)
```bash
PUT /api/products/507f1f77bcf86cd799439011
Authorization: Bearer <admin_token>
Content-Type: multipart/form-data
```

#### 7. Delete Product (Admin)
```bash
DELETE /api/products/507f1f77bcf86cd799439011
Authorization: Bearer <admin_token>
```

### Category Endpoints

#### 1. Get All Categories
```bash
GET /api/categories?gender=boy&ageRange=5-8&season=summer
```

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "_id": "507f1f77bcf86cd799439012",
      "name": {
        "uz": "Ko'ylaklar",
        "en": "T-Shirts",
        "ru": "Футболки"
      },
      "description": {
        "uz": "O'g'il bolalar ko'ylaklari",
        "en": "Boys T-Shirts",
        "ru": "Футболки для мальчиков"
      },
      "slug": "ko'ylaklar",
      "parent": null,
      "order": 1,
      "gender": "boy",
      "ageRanges": ["5-8", "8-12"],
      "season": "summer",
      "image": "/uploads/category1.jpg",
      "isActive": true
    }
  ]
}
```

#### 2. Get Category Tree
```bash
GET /api/categories/tree
```

#### 3. Get Category by Slug
```bash
GET /api/categories/slug/ko'ylaklar
```

#### 4. Create Category (Admin)
```bash
POST /api/categories
Authorization: Bearer <admin_token>
Content-Type: multipart/form-data

{
  "name": {
    "uz": "Yangi kategoriya",
    "en": "New Category",
    "ru": "Новая категория"
  },
  "description": {
    "uz": "Tavsif",
    "en": "Description",
    "ru": "Описание"
  },
  "parent": "507f1f77bcf86cd799439012",
  "order": 1,
  "gender": "boy",
  "ageRanges": ["5-8", "8-12"],
  "season": "summer",
  "image": File
}
```

### Cart Endpoints

#### 1. Get Cart
```bash
GET /api/cart
Authorization: Bearer <token>
```

**Response:**
```json
{
  "success": true,
  "data": {
    "items": [
      {
        "productId": "507f1f77bcf86cd799439011",
        "product": {
          "_id": "507f1f77bcf86cd799439011",
          "title": {
            "uz": "Ko'ylak",
            "en": "T-Shirt",
            "ru": "Футболка"
          },
          "price": 150000,
          "images": ["/uploads/product1.jpg"]
        },
        "quantity": 2,
        "size": "M",
        "price": 150000,
        "totalPrice": 300000
      }
    ],
    "totalItems": 2,
    "subtotal": 300000,
    "discount": 0,
    "totalAmount": 300000,
    "appliedPromoCode": null
  }
}
```

#### 2. Add to Cart
```bash
POST /api/cart/add
Authorization: Bearer <token>
Content-Type: application/json

{
  "productId": "507f1f77bcf86cd799439011",
  "quantity": 2,
  "size": "M"
}
```

#### 3. Update Cart Item
```bash
PUT /api/cart/update
Authorization: Bearer <token>
Content-Type: application/json

{
  "productId": "507f1f77bcf86cd799439011",
  "quantity": 3,
  "size": "M"
}
```

#### 4. Remove from Cart
```bash
DELETE /api/cart/remove
Authorization: Bearer <token>
Content-Type: application/json

{
  "productId": "507f1f77bcf86cd799439011",
  "size": "M"
}
```

#### 5. Clear Cart
```bash
DELETE /api/cart/clear
Authorization: Bearer <token>
```

#### 6. Apply Promo Code
```bash
POST /api/cart/apply-promo
Authorization: Bearer <token>
Content-Type: application/json

{
  "code": "SUMMER20"
}
```

#### 7. Remove Promo Code
```bash
DELETE /api/cart/remove-promo
Authorization: Bearer <token>
```

#### 8. Validate Cart
```bash
GET /api/cart/validate
Authorization: Bearer <token>
```

### Order Endpoints

#### 1. Create Order
```bash
POST /api/orders
Authorization: Bearer <token>
Content-Type: application/json

{
  "shippingAddress": {
    "fullName": "John Doe",
    "phone": "+998901234567",
    "street": "123 Main St",
    "city": "Tashkent",
    "state": "Tashkent",
    "zipCode": "100000"
  },
  "paymentMethod": "cash_on_delivery",
  "deliveryTimePreference": "afternoon",
  "priority": "normal"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "_id": "507f1f77bcf86cd799439013",
    "user": "507f1f77bcf86cd799439011",
    "items": [
      {
        "productId": "507f1f77bcf86cd799439011",
        "quantity": 2,
        "size": "M",
        "price": 150000,
        "totalPrice": 300000
      }
    ],
    "totalAmount": 300000,
    "status": "pending",
    "shippingAddress": {
      "fullName": "John Doe",
      "phone": "+998901234567",
      "street": "123 Main St",
      "city": "Tashkent",
      "state": "Tashkent",
      "zipCode": "100000"
    },
    "paymentMethod": "cash_on_delivery",
    "createdAt": "2024-01-01T00:00:00.000Z"
  }
}
```

#### 2. Get User Orders
```bash
GET /api/orders?status=delivered&page=1&limit=10
Authorization: Bearer <token>
```

#### 3. Get Single Order
```bash
GET /api/orders/507f1f77bcf86cd799439013
Authorization: Bearer <token>
```

#### 4. Cancel Order
```bash
PUT /api/orders/507f1f77bcf86cd799439013/cancel
Authorization: Bearer <token>
Content-Type: application/json

{
  "reason": "Changed my mind"
}
```

#### 5. Request Return
```bash
PUT /api/orders/507f1f77bcf86cd799439013/return
Authorization: Bearer <token>
Content-Type: application/json

{
  "reason": "Wrong size"
}
```

#### 6. Get All Orders (Admin)
```bash
GET /api/orders/all?status=pending&startDate=2024-01-01&endDate=2024-12-31&page=1&limit=20
Authorization: Bearer <admin_token>
```

#### 7. Update Order Status (Admin)
```bash
PUT /api/orders/507f1f77bcf86cd799439013/status
Authorization: Bearer <admin_token>
Content-Type: application/json

{
  "status": "confirmed",
  "note": "Order confirmed and ready for processing"
}
```

#### 8. Assign Courier (Admin)
```bash
PUT /api/orders/507f1f77bcf86cd799439013/assign-courier
Authorization: Bearer <admin_token>
Content-Type: application/json

{
  "courierId": "507f1f77bcf86cd799439014",
  "estimatedPickupTime": "2024-01-01T10:00:00.000Z",
  "estimatedDeliveryTime": "2024-01-01T14:00:00.000Z"
}
```

### User Management (Admin)

#### 1. Get All Users
```bash
GET /api/users/admin/all?role=user&isActive=true&search=john&page=1&limit=20
Authorization: Bearer <admin_token>
```

#### 2. Get User by ID
```bash
GET /api/users/admin/507f1f77bcf86cd799439011
Authorization: Bearer <admin_token>
```

#### 3. Create User
```bash
POST /api/users/admin
Authorization: Bearer <admin_token>
Content-Type: application/json

{
  "firstName": "Jane",
  "lastName": "Doe",
  "email": "jane@example.com",
  "phone": "+998901234568",
  "password": "password123",
  "role": "user",
  "isActive": true
}
```

#### 4. Update User
```bash
PUT /api/users/admin/507f1f77bcf86cd799439011
Authorization: Bearer <admin_token>
Content-Type: application/json

{
  "fullName": "Jane Updated",
  "email": "jane.updated@example.com",
  "phone": "+998901234568",
  "role": "admin",
  "isActive": true
}
```

#### 5. Activate User
```bash
PUT /api/users/admin/507f1f77bcf86cd799439011/activate
Authorization: Bearer <admin_token>
```

#### 6. Delete User
```bash
DELETE /api/users/admin/507f1f77bcf86cd799439011
Authorization: Bearer <admin_token>
```

### Admin Dashboard

#### 1. Get Dashboard Stats
```bash
GET /api/admin/dashboard?startDate=2024-01-01&endDate=2024-12-31
Authorization: Bearer <admin_token>
```

**Response:**
```json
{
  "success": true,
  "data": {
    "totalUsers": 1500,
    "totalOrders": 2500,
    "totalRevenue": 150000000,
    "recentOrders": [
      {
        "_id": "507f1f77bcf86cd799439013",
        "totalAmount": 300000,
        "status": "delivered",
        "createdAt": "2024-01-01T00:00:00.000Z"
      }
    ],
    "monthlyStats": {
      "users": [100, 120, 150, 180],
      "orders": [200, 250, 300, 350],
      "revenue": [10000000, 12000000, 15000000, 18000000]
    }
  }
}
```

#### 2. Get Financial Stats
```bash
GET /api/admin/finance?startDate=2024-01-01&endDate=2024-12-31
Authorization: Bearer <admin_token>
```

#### 3. Get Analytics
```bash
GET /api/admin/analytics
Authorization: Bearer <admin_token>
```

### Banner Endpoints

#### 1. Get Banners
```bash
GET /api/banners
```

#### 2. Create Banner (Admin)
```bash
POST /api/banners
Authorization: Bearer <admin_token>
Content-Type: multipart/form-data

{
  "title": "Summer Sale",
  "description": "Get 20% off on summer collection",
  "link": "/products?category=summer",
  "isActive": true,
  "image": File
}
```

### Promo Code Endpoints

#### 1. Get Promo Codes (Admin)
```bash
GET /api/promos
Authorization: Bearer <admin_token>
```

#### 2. Create Promo Code (Admin)
```bash
POST /api/promos
Authorization: Bearer <admin_token>
Content-Type: application/json

{
  "code": "SUMMER20",
  "discount": 20,
  "type": "percentage",
  "minAmount": 100000,
  "maxDiscount": 50000,
  "usageLimit": 100,
  "validFrom": "2024-06-01T00:00:00.000Z",
  "validUntil": "2024-08-31T23:59:59.000Z",
  "isActive": true
}
```

### Contact Endpoints

#### 1. Send Contact Message
```bash
POST /api/contact
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john@example.com",
  "phone": "+998901234567",
  "subject": "General Inquiry",
  "message": "I have a question about your products"
}
```

### Notification Endpoints

#### 1. Get Notifications
```bash
GET /api/notifications
Authorization: Bearer <token>
```

#### 2. Mark as Read
```bash
PUT /api/notifications/507f1f77bcf86cd799439015/read
Authorization: Bearer <token>
```

## Filters & Query Parameters

### Product Filters

```javascript
// Basic filters
{
  category: "string",        // Category ID
  search: "string",          // Search term
  page: "number",           // Page number (default: 1)
  limit: "number",          // Items per page (default: 10)
  
  // Product attributes
  gender: "boy|girl|unisex", // Gender
  ageRange: "0-2|2-5|5-8|8-12|12-16", // Age range
  season: "spring|summer|autumn|winter|all_season", // Season
  status: "active|inactive|out_of_stock", // Status
  
  // Price filters
  minPrice: "number",       // Minimum price
  maxPrice: "number",       // Maximum price
  
  // Size filters
  sizes: "XS,S,M,L,XL,XXL", // Sizes (comma separated)
  
  // Sorting
  sortBy: "price|name|createdAt|updatedAt", // Sort field
  sortOrder: "asc|desc"     // Sort order
}
```

### Order Filters

```javascript
{
  // Pagination
  page: "number",           // Page number (default: 1)
  limit: "number",          // Orders per page (default: 10)
  
  // Order status
  status: "pending|confirmed|processing|ready_for_pickup|assigned_to_courier|picked_up|on_the_way|out_for_delivery|delivered|cancelled|returned",
  
  // Date filters
  startDate: "YYYY-MM-DD",  // Start date
  endDate: "YYYY-MM-DD",    // End date
  
  // Amount filters
  minAmount: "number",      // Minimum amount
  maxAmount: "number",      // Maximum amount
  
  // Sorting
  sortBy: "createdAt|totalAmount|status", // Sort field
  sortOrder: "asc|desc"     // Sort order
}
```

### User Filters (Admin)

```javascript
{
  // Pagination
  page: "number",           // Page number (default: 1)
  limit: "number",          // Users per page (default: 10)
  
  // User attributes
  role: "user|admin|superadmin", // Role
  isActive: "boolean",      // Active users
  
  // Search
  search: "string",         // Search by name, phone, or email
  
  // Date filters
  startDate: "YYYY-MM-DD",  // Registration date
  endDate: "YYYY-MM-DD",    // Registration date
  
  // Sorting
  sortBy: "createdAt|fullName|email", // Sort field
  sortOrder: "asc|desc"     // Sort order
}
```

## Response Formats

### Success Response

```json
{
  "success": true,
  "message": "Operation completed successfully",
  "data": {
    // Response data
  },
  "pagination": {
    "currentPage": 1,
    "totalPages": 5,
    "totalItems": 100,
    "itemsPerPage": 20,
    "hasNextPage": true,
    "hasPrevPage": false
  }
}
```

### Error Response

```json
{
  "success": false,
  "error": "Validation Error",
  "message": "Invalid input data",
  "details": {
    "field": "email",
    "value": "invalid-email",
    "allowedValues": ["valid email format"]
  },
  "statusCode": 400
}
```

### Common HTTP Status Codes

- **200:** Success
- **201:** Created
- **400:** Bad Request
- **401:** Unauthorized
- **403:** Forbidden
- **404:** Not Found
- **422:** Validation Error
- **500:** Internal Server Error

## Error Handling

### Validation Errors

```json
{
  "success": false,
  "error": "Validation Error",
  "message": "Invalid input data",
  "details": [
    {
      "field": "phone",
      "message": "Phone number must be in format +998XXXXXXXXX"
    },
    {
      "field": "password",
      "message": "Password must be at least 6 characters long"
    }
  ],
  "statusCode": 422
}
```

### Authentication Errors

```json
{
  "success": false,
  "error": "Authentication Error",
  "message": "Invalid or expired token",
  "statusCode": 401
}
```

### Authorization Errors

```json
{
  "success": false,
  "error": "Authorization Error",
  "message": "Insufficient permissions",
  "statusCode": 403
}
```

### Not Found Errors

```json
{
  "success": false,
  "error": "Not Found",
  "message": "Product not found",
  "statusCode": 404
}
```

## Frontend Integration

### JavaScript/TypeScript Integration

#### 1. API Client Setup

```javascript
// api.js
import axios from 'axios';

const API_BASE_URL = process.env.NODE_ENV === 'production' 
  ? 'https://api.faskids.shop/api' 
  : 'http://localhost:5001/api';

const api = axios.create({
  baseURL: API_BASE_URL,
  timeout: 10000,
  headers: {
    'Content-Type': 'application/json',
  },
});

// Request interceptor to add auth token
api.interceptors.request.use(
  (config) => {
    const token = localStorage.getItem('token');
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  (error) => {
    return Promise.reject(error);
  }
);

// Response interceptor for error handling
api.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response?.status === 401) {
      // Handle unauthorized access
      localStorage.removeItem('token');
      window.location.href = '/login';
    }
    return Promise.reject(error);
  }
);

export default api;
```

#### 2. Authentication Service

```javascript
// authService.js
import api from './api';

export const authService = {
  // Login
  async login(phone, password) {
    const response = await api.post('/auth/login', { phone, password });
    if (response.data.success) {
      localStorage.setItem('token', response.data.token);
      localStorage.setItem('user', JSON.stringify(response.data.user));
    }
    return response.data;
  },

  // Register
  async register(userData) {
    const response = await api.post('/auth/register', userData);
    if (response.data.success) {
      localStorage.setItem('token', response.data.token);
      localStorage.setItem('user', JSON.stringify(response.data.user));
    }
    return response.data;
  },

  // Logout
  async logout() {
    try {
      await api.post('/auth/logout');
    } catch (error) {
      console.error('Logout error:', error);
    } finally {
      localStorage.removeItem('token');
      localStorage.removeItem('user');
      window.location.href = '/login';
    }
  },

  // Get current user
  getCurrentUser() {
    const user = localStorage.getItem('user');
    return user ? JSON.parse(user) : null;
  },

  // Check if user is authenticated
  isAuthenticated() {
    return !!localStorage.getItem('token');
  }
};
```

#### 3. Product Service

```javascript
// productService.js
import api from './api';

export const productService = {
  // Get all products with filters
  async getProducts(filters = {}) {
    const params = new URLSearchParams();
    
    Object.entries(filters).forEach(([key, value]) => {
      if (value !== undefined && value !== null && value !== '') {
        params.append(key, value);
      }
    });

    const response = await api.get(`/products?${params.toString()}`);
    return response.data;
  },

  // Get single product
  async getProduct(id) {
    const response = await api.get(`/products/${id}`);
    return response.data;
  },

  // Search products
  async searchProducts(query, category = null) {
    const params = new URLSearchParams({ q: query });
    if (category) params.append('category', category);
    
    const response = await api.get(`/products/search?${params.toString()}`);
    return response.data;
  },

  // Get related products
  async getRelatedProducts(id) {
    const response = await api.get(`/products/${id}/related`);
    return response.data;
  }
};
```

#### 4. Cart Service

```javascript
// cartService.js
import api from './api';

export const cartService = {
  // Get cart
  async getCart() {
    const response = await api.get('/cart');
    return response.data;
  },

  // Add to cart
  async addToCart(productId, quantity, size) {
    const response = await api.post('/cart/add', {
      productId,
      quantity,
      size
    });
    return response.data;
  },

  // Update cart item
  async updateCartItem(productId, quantity, size) {
    const response = await api.put('/cart/update', {
      productId,
      quantity,
      size
    });
    return response.data;
  },

  // Remove from cart
  async removeFromCart(productId, size) {
    const response = await api.delete('/cart/remove', {
      data: { productId, size }
    });
    return response.data;
  },

  // Clear cart
  async clearCart() {
    const response = await api.delete('/cart/clear');
    return response.data;
  },

  // Apply promo code
  async applyPromoCode(code) {
    const response = await api.post('/cart/apply-promo', { code });
    return response.data;
  },

  // Remove promo code
  async removePromoCode() {
    const response = await api.delete('/cart/remove-promo');
    return response.data;
  }
};
```

#### 5. Order Service

```javascript
// orderService.js
import api from './api';

export const orderService = {
  // Create order
  async createOrder(orderData) {
    const response = await api.post('/orders', orderData);
    return response.data;
  },

  // Get user orders
  async getUserOrders(filters = {}) {
    const params = new URLSearchParams();
    
    Object.entries(filters).forEach(([key, value]) => {
      if (value !== undefined && value !== null && value !== '') {
        params.append(key, value);
      }
    });

    const response = await api.get(`/orders?${params.toString()}`);
    return response.data;
  },

  // Get single order
  async getOrder(id) {
    const response = await api.get(`/orders/${id}`);
    return response.data;
  },

  // Cancel order
  async cancelOrder(id, reason) {
    const response = await api.put(`/orders/${id}/cancel`, { reason });
    return response.data;
  },

  // Request return
  async requestReturn(id, reason) {
    const response = await api.put(`/orders/${id}/return`, { reason });
    return response.data;
  }
};
```

#### 6. User Service

```javascript
// userService.js
import api from './api';

export const userService = {
  // Get profile
  async getProfile() {
    const response = await api.get('/users/profile');
    return response.data;
  },

  // Update profile
  async updateProfile(profileData) {
    const response = await api.put('/users/profile', profileData);
    return response.data;
  },

  // Get user orders
  async getUserOrders(filters = {}) {
    const params = new URLSearchParams();
    
    Object.entries(filters).forEach(([key, value]) => {
      if (value !== undefined && value !== null && value !== '') {
        params.append(key, value);
      }
    });

    const response = await api.get(`/users/orders?${params.toString()}`);
    return response.data;
  },

  // Get user stats
  async getUserStats() {
    const response = await api.get('/users/stats');
    return response.data;
  }
};
```

### React Integration Example

```jsx
// ProductList.jsx
import React, { useState, useEffect } from 'react';
import { productService } from '../services/productService';

const ProductList = () => {
  const [products, setProducts] = useState([]);
  const [loading, setLoading] = useState(true);
  const [filters, setFilters] = useState({
    category: '',
    gender: '',
    ageRange: '',
    minPrice: '',
    maxPrice: '',
    page: 1,
    limit: 20
  });

  useEffect(() => {
    loadProducts();
  }, [filters]);

  const loadProducts = async () => {
    try {
      setLoading(true);
      const response = await productService.getProducts(filters);
      setProducts(response.data.products);
    } catch (error) {
      console.error('Error loading products:', error);
    } finally {
      setLoading(false);
    }
  };

  const handleFilterChange = (key, value) => {
    setFilters(prev => ({
      ...prev,
      [key]: value,
      page: 1 // Reset to first page when filters change
    }));
  };

  if (loading) return <div>Loading...</div>;

  return (
    <div>
      {/* Filter controls */}
      <div className="filters">
        <select 
          value={filters.category} 
          onChange={(e) => handleFilterChange('category', e.target.value)}
        >
          <option value="">All Categories</option>
          {/* Category options */}
        </select>
        
        <select 
          value={filters.gender} 
          onChange={(e) => handleFilterChange('gender', e.target.value)}
        >
          <option value="">All Genders</option>
          <option value="boy">Boys</option>
          <option value="girl">Girls</option>
          <option value="unisex">Unisex</option>
        </select>
        
        <input
          type="number"
          placeholder="Min Price"
          value={filters.minPrice}
          onChange={(e) => handleFilterChange('minPrice', e.target.value)}
        />
        
        <input
          type="number"
          placeholder="Max Price"
          value={filters.maxPrice}
          onChange={(e) => handleFilterChange('maxPrice', e.target.value)}
        />
      </div>

      {/* Product grid */}
      <div className="products-grid">
        {products.map(product => (
          <div key={product._id} className="product-card">
            <img src={product.images[0]} alt={product.title.en} />
            <h3>{product.title.en}</h3>
            <p>${product.price}</p>
            <button onClick={() => addToCart(product._id)}>
              Add to Cart
            </button>
          </div>
        ))}
      </div>
    </div>
  );
};

export default ProductList;
```

### Error Handling in Frontend

```javascript
// errorHandler.js
export const handleApiError = (error) => {
  if (error.response) {
    const { status, data } = error.response;
    
    switch (status) {
      case 400:
        return `Bad Request: ${data.message}`;
      case 401:
        // Redirect to login
        window.location.href = '/login';
        return 'Please login to continue';
      case 403:
        return 'Access denied. Insufficient permissions.';
      case 404:
        return 'Resource not found.';
      case 422:
        return `Validation Error: ${data.message}`;
      case 500:
        return 'Server error. Please try again later.';
      default:
        return 'An unexpected error occurred.';
    }
  } else if (error.request) {
    return 'Network error. Please check your connection.';
  } else {
    return 'An error occurred while processing your request.';
  }
};

// Usage in components
try {
  const response = await productService.getProducts(filters);
  setProducts(response.data.products);
} catch (error) {
  const errorMessage = handleApiError(error);
  setError(errorMessage);
}
```

## Examples

### Complete Product Search Example

```javascript
// Search products with multiple filters
const searchProducts = async () => {
  try {
    const filters = {
      category: '507f1f77bcf86cd799439012',
      gender: 'boy',
      ageRange: '5-8',
      minPrice: 100000,
      maxPrice: 500000,
      sortBy: 'price',
      sortOrder: 'asc',
      page: 1,
      limit: 20
    };

    const response = await productService.getProducts(filters);
    console.log('Products:', response.data.products);
    console.log('Pagination:', response.data.pagination);
  } catch (error) {
    console.error('Search error:', error);
  }
};
```

### Complete Order Creation Example

```javascript
// Create order with cart items
const createOrder = async () => {
  try {
    const orderData = {
      shippingAddress: {
        fullName: 'John Doe',
        phone: '+998901234567',
        street: '123 Main St',
        city: 'Tashkent',
        state: 'Tashkent',
        zipCode: '100000'
      },
      paymentMethod: 'cash_on_delivery',
      deliveryTimePreference: 'afternoon',
      priority: 'normal'
    };

    const response = await orderService.createOrder(orderData);
    console.log('Order created:', response.data);
    
    // Clear cart after successful order
    await cartService.clearCart();
    
  } catch (error) {
    console.error('Order creation error:', error);
  }
};
```

### Complete Authentication Flow

```javascript
// Login and redirect
const handleLogin = async (phone, password) => {
  try {
    const response = await authService.login(phone, password);
    
    if (response.success) {
      // Redirect to dashboard or home page
      window.location.href = '/dashboard';
    } else {
      setError(response.message);
    }
  } catch (error) {
    const errorMessage = handleApiError(error);
    setError(errorMessage);
  }
};

// Protected route component
const ProtectedRoute = ({ children }) => {
  const isAuthenticated = authService.isAuthenticated();
  
  if (!isAuthenticated) {
    return <Navigate to="/login" />;
  }
  
  return children;
};
```

This comprehensive documentation covers all aspects of the FAS KIDS API, including authentication, endpoints, filters, response formats, error handling, and frontend integration examples. 
