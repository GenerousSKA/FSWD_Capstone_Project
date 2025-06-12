SMARTSTOCK: INVENTORY MANAGEMENT SYSTEM – CAPSTONE PROJECT PROPOSAL

Project Title: SmartStock: Advanced Inventory Management System

Problem Statement
Managing inventory efficiently is crucial for businesses, but many small and medium-sized enterprises (SMEs) still rely on manual tracking or outdated systems, leading to stock discrepancies, lost sales, and operational inefficiencies.

Why is this problem significant?
•	Manual inventory tracking is error-prone and time-consuming.
•	Poor stock visibility can lead to overstocking or stockouts.
•	Lack of real-time data makes decision-making difficult.
•	Existing solutions may be too complex or expensive for small businesses.

This project aims to provide a user-friendly, cost-effective inventory management system that helps businesses track stock levels, manage suppliers, and generate reports—all in one place.

Overview of the Application’s Functionality
Smart Stock: Is a full-stack web application that allows businesses to:
•	Track inventory in real-time with stock level alerts.
•	Manage suppliers and purchase orders.
•	Generate reports (sales trends, stock valuation, etc.).
•	Control user access with role-based permissions (Admin, Manager, Staff).
•	Scan barcodes for quick product lookup.

The system will have:
•	A React.js frontend with a clean, responsive UI.
•	A Node.js + Express backend with a MongoDB database.
•	JWT authentication for secure login.
•	RESTful APIs for data exchange.

Technology Stack
Category	Technologies
Frontend	React.js, TypeScript, Material-UI, React Query, Formik + Yup
Backend	Node.js, Express.js, MongoDB (Mongoose), JWT
DevOps	Git, GitHub, Vercel (Frontend Hosting), Render (Backend Hosting)
Testing	Jest (Unit Testing), Postman (API Testing)
Additional Tools	Barcode.js (for barcode generation), Chart.js (for analytics)

Features to be Implemented
Core Features (MVP - Minimum Viable Product)
Product Management – Add, edit, delete, and categorize products.
Inventory Tracking – Real-time stock updates with low-stock alerts.
Supplier Management – Track supplier details and purchase orders.
User Authentication – Login, logout, and role-based access (Admin, Staff).
Basic Dashboard – Summary of stock levels and recent activities.

Additional Features (Future Enhancements)
🔹 Barcode/QR Scanning – Scan products using a webcam or mobile.
🔹 Advanced Reporting – Export data to Excel/PDF, sales analytics.
🔹 Multi-Warehouse Support – Track inventory across multiple locations.
🔹 Mobile App Integration – Sync with a React Native app.

User Stories
1.	As an Admin, I want to add new products to the inventory so that I can keep track of available stock.
2.	As a Store Manager, I want to receive low-stock alerts so that I can reorder products before they run out.
3.	As a Staff Member, I want to update stock levels after sales so that inventory records stay accurate.
4.	As a Supplier, I want to view purchase orders so that I know what to deliver.
5.	As a Business Owner, I want to generate sales reports so that I can analyze business performance.



MILESTONE TWO ASSIGNMENT: DESIGN & API CONTRACT DEVELOPMENT

Objective: Design the application’s architecture, database schema, and develop API contracts.

High-Level Architecture
Components Breakdown
Frontend: React SPA with Material-UI
API Gateway: Express.js router
Microservices:
o	Auth Service (JWT)
o	Inventory Service
o	Reporting Service
Database: MongoDB Atlas
External Integrations:
o	Barcode Scanner API
o	Email/SMS Alert Service


![Architecture Image](https://github.com/user-attachments/assets/3ceae7ac-b587-412f-8166-ec8f3304462b)


Database Schema Design
o	Define the necessary tables, relationships, and entities for the app.
o	Create ER diagrams (entity-relationship) and discuss CRUD operations.


![ERDiagram](https://github.com/user-attachments/assets/dab03501-9cdb-4758-92a8-03b89aa8c2b5)


erDiagram
    USER ||--o{ PRODUCT : creates
    USER ||--o{ SUPPLIER : manages
    PRODUCT }|--|| SUPPLIER : supplied_by
    PRODUCT ||--o{ STOCK_MOVEMENT : has
    PRODUCT {
        string _id PK
        string name
        string sku
        string category
        number currentStock
        number lowStockThreshold
        date expiryDate
        objectId supplierId FK
    }
    
SUPPLIER {
        string _id PK
        string name
        string contactEmail
        string phone
    }
    
STOCK_MOVEMENT {
        string _id PK
        objectId productId FK
        number quantity
        string movementType
        date timestamp
    }
    
USER {
        string _id PK
        string email
        string passwordHash
        string role
    }


Collections Description
Collection	Fields	Relationships
products	_id, name, sku, category, currentStock, expiryDate, supplierId	Belongs to supplier
suppliers	_id, name, contactEmail, phone	Has many products
stock_movements	_id, productId, quantity, movementType, timestamp	References product
users	_id, email, passwordHash, role (admin/manager/staff)	



API Contract
o	Define the API endpoints, request/response format, and authorization needs.

Base URL: https://api.smartstock.in/v1
Products

GET /products
# Response
{
  "data": [
    {
      "_id": "prod123",
      "name": "Organic Milk",
      "currentStock": 42,
      "lowStockThreshold": 20,
      "expiryDate": "2023-12-31"
    }
  ]
}

POST /products
# Request
{
  "name": "New Product",
  "sku": "SKU123",
  "initialStock": 100
}

Inventory

PATCH /inventory/{productId}/adjust
# Request
{
  "adjustment": -5,
  "reason": "sale"
}

Reporting

GET /reports/expiry-alerts
# Response
{
  "critical": [
    { "product": "Yogurt", "daysUntilExpiry": 2 }



MILESTONE PROJECT: DEVELOPMENT – BACKEND IMPLEMENTATION 2 WEEKS
Objective:
Start implementing the core functionalities of the application, focusing on backend development first.
________________________________________
First, Install These on Your Computer:
•	Node.js (LTS version)
•	MongoDB Community Server
•	Postman (for API testing)
InitializeNode.jsProject
•	mkdir smartstock-backend
•	cd smartstock-backend
•	npm init -y
________________________________________
InstallRequiredDependencies
•	npm install express mongoose body-parser cors dotenv
•	npm install --save-dev nodemon

o	express: Web framework for Node.js
o	mongoose: MongoDB object modeling
o	body-parser: Parse incoming request bodies
o	cors: Enable Cross-Origin Resource Sharing
o	dotenv: Load environment variables
o	nodemon: Automatically restart server during development
________________________________________
CreateExpressServer (server.js)
// Load environment variables
require('dotenv').config();

// Import required packages
const express = require('express');
const mongoose = require('mongoose');
const bodyParser = require('body-parser');
const cors = require('cors');

// Create Express app
const app = express();

// Middleware
app.use(cors()); // Enable CORS
app.use(bodyParser.json()); // Parse JSON bodies

// Database connection
mongoose.connect(process.env.MONGODB_URI, {
    useNewUrlParser: true,
    useUnifiedTopology: true
})
.then(() => console.log('Connected to MongoDB'))
.catch(err => console.error('MongoDB connection error:', err));

// Simple test route
app.get('/', (req, res) => {
    res.send('SmartStock Backend is running!');
});

// Set the port
const PORT = process.env.PORT || 5000;

// Start the server
app.listen(PORT, () => {
    console.log(`Server running on port ${PORT}`);
});
________________________________________
Configure Environment Variables (.env)
MONGODB_URI=mongodb://localhost:27017/smartstock
PORT=5000
________________________________________
Set Up Development Script (package.json)
"scripts": {
  "start": "node server.js",
  "dev": "nodemon server.js"
}

Now you can run the server with: npm run dev

Your server should be running at http://localhost:5000
________________________________________
Database Integration with MongoDB
Create the Models Folder
mkdir models
________________________________________
CreateTheProductModel (models/Product.js)
const mongoose = require('mongoose');
const productSchema = new mongoose.Schema({
    name: {
        type: String,
        required: [true, 'Product name is required'],
        trim: true
    },
    description: {
        type: String,
        trim: true
    },
    sku: {
        type: String,
        required: [true, 'SKU is required'],
        unique: true,
        trim: true
    },
    category: {
        type: String,
        required: [true, 'Category is required']
    },
    price: {
        type: Number,
        required: [true, 'Price is required'],
        min: [0, 'Price cannot be negative']
    },
    cost: {
        type: Number,
        min: [0, 'Cost cannot be negative']
    },
    reorderLevel: {
        type: Number,
        default: 5,
        min: [0, 'Reorder level cannot be negative']
    },
    createdAt: {
        type: Date,
        default: Date.now
    },
    updatedAt: {
        type: Date,
        default: Date.now
    }
});

// Update the updatedAt field before saving
productSchema.pre('save', function(next) {
    this.updatedAt = Date.now();
    next();
});

module.exports = mongoose.model('Product', productSchema);
________________________________________

CreateTheInventoryModel (models/Inventory.js)

const mongoose = require('mongoose');

const inventorySchema = new mongoose.Schema({
    product: {
        type: mongoose.Schema.Types.ObjectId,
        ref: 'Product',
        required: [true, 'Product reference is required']
    },
    quantity: {
        type: Number,
        required: [true, 'Quantity is required'],
        min: [0, 'Quantity cannot be negative']
    },
    location: {
        type: String,
        required: [true, 'Location is required']
    },
    lastUpdated: {
        type: Date,
        default: Date.now
    },
    status: {
        type: String,
        enum: ['in-stock', 'low-stock', 'out-of-stock'],
        default: 'in-stock'
    }
});

module.exports = mongoose.model('Inventory', inventorySchema);
________________________________________

DevelopingAPIRoutes

Create the Routes and Controllers Structure

mkdir routes controllers

Create Product Routes (routes/products.js)

const express = require('express');
const router = express.Router();
const {
    createProduct,
    getAllProducts,
    getProductById,
    updateProduct,
    deleteProduct
} = require('../controllers/productController');

// Product CRUD routes
router.post('/', createProduct);
router.get('/', getAllProducts);
router.get('/:id', getProductById);
router.put('/:id', updateProduct);
router.delete('/:id', deleteProduct);

module.exports = router;
________________________________________


CreateProductController (controllers/productController.js)
const Product = require('../models/Product');

// Create a new product
exports.createProduct = async (req, res) => {
    try {
        const product = new Product(req.body);
        await product.save();
        res.status(201).json({
            success: true,
            data: product
        });
    } catch (error) {
        res.status(400).json({
            success: false,
            error: error.message
        });
    }
};

// Get all products
exports.getAllProducts = async (req, res) => {
    try {
        const products = await Product.find();
        res.status(200).json({
            success: true,
            count: products.length,
            data: products
        });
    } catch (error) {
        res.status(500).json({
            success: false,
            error: error.message
        });
    }
};

// Get product by ID
exports.getProductById = async (req, res) => {
    try {
        const product = await Product.findById(req.params.id);
        
        if (!product) {
            return res.status(404).json({
                success: false,
                error: 'Product not found'
            });
        }
        
        res.status(200).json({
            success: true,
            data: product
        });
    } catch (error) {
        res.status(500).json({
            success: false,
            error: error.message
        });
    }
};

// Update product
exports.updateProduct = async (req, res) => {
    try {
        const product = await Product.findByIdAndUpdate(
            req.params.id,
            req.body,
            { 
                new: true,
                runValidators: true
            }
        );
        
        if (!product) {
            return res.status(404).json({
                success: false,
                error: 'Product not found'
            });
        }
        
        res.status(200).json({
            success: true,
            data: product
        });
    } catch (error) {
        res.status(400).json({
            success: false,
            error: error.message
        });
    }
};

// Delete product
exports.deleteProduct = async (req, res) => {
    try {
        const product = await Product.findByIdAndDelete(req.params.id);
        
        if (!product) {
            return res.status(404).json({
                success: false,
                error: 'Product not found'
            });
        }
        
        res.status(200).json({
            success: true,
            data: {}
        });
    } catch (error) {
        res.status(500).json({
            success: false,
            error: error.message
        });
    }
};
________________________________________

TestingYourAPIWithPostman

Create a Product:
•	Method: POST
•	URL: http://localhost:5000/api/products
•	Headers:
o	Key: Content-Type
o	Value: application/json
•	Body (raw, JSON):

{
    "name": "Wireless Mouse",
    "description": "Ergonomic wireless mouse with 2.4GHz connection",
    "sku": "WM-1001",
    "category": "Computer Accessories",
    "price": 24.99,
    "cost": 12.50,
    "reorderLevel": 10
}


 

Get All Products:
•	Method: GET
•	URL: http://localhost:5000/api/products

 

Get Single Product:
•	Method: GET
•	URL: http://localhost:5000/api/products/ID

 

Update Product:
•	Method: PUT
•	URL: http://localhost:5000/api/products/[ID]
•	Body (raw, JSON):

 

  ],
  "warning": []
}


