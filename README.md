# SOLVAYLA-MAEKET
const express = require('express');
const mongoose = require('mongoose');
const cors = require('cors');
const bodyParser = require('body-parser');
const path = require('path');

// Initialize app
const app = express();
app.use(cors());
app.use(bodyParser.json());

// Connect to MongoDB
mongoose.connect('mongodb://localhost:27017/solvayla', { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => console.log('Connected to MongoDB'))
  .catch(err => console.log('Error connecting to MongoDB:', err));

// Sample Product Model (for demonstration)
const productSchema = new mongoose.Schema({
  name: String,
  category: String,
  price: Number,
  image: String,
  description: String
});

const Product = mongoose.model('Product', productSchema);

// Routes for product fetching
app.get('/products', async (req, res) => {
  try {
    const products = await Product.find();
    res.json(products);
  } catch (err) {
    res.status(500).send(err);
  }
});

// Serve static HTML, CSS, and JS
app.get('/', (req, res) => {
  res.send(`
    <!DOCTYPE html>
    <html lang="en">
    <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Solvayla - Your Ultimate E-Commerce Store</title>
      <style>
        /* Global Styles */
        * {
          margin: 0;
          padding: 0;
          box-sizing: border-box;
        }

        body {
          font-family: 'Roboto', sans-serif;
          background-color: #f5f5f5;
        }

        header {
          background-color: #333;
          color: #fff;
          padding: 20px 0;
        }

        .navbar {
          display: flex;
          justify-content: space-between;
          align-items: center;
        }

        .navbar .logo a {
          color: #fff;
          font-size: 1.8rem;
          text-decoration: none;
        }

        .navbar .nav-links {
          list-style: none;
          display: flex;
        }

        .navbar .nav-links li {
          margin: 0 20px;
        }

        .navbar .nav-links a {
          color: #fff;
          text-decoration: none;
          font-weight: 500;
          font-size: 1rem;
        }

        .navbar .hamburger-menu {
          display: none;
        }

        .hero {
          background-image: url('https://via.placeholder.com/1500x500'); /* Example Image */
          background-size: cover;
          background-position: center;
          height: 400px;
          display: flex;
          justify-content: center;
          align-items: center;
          text-align: center;
          color: white;
        }

        .hero h1 {
          font-size: 3rem;
          font-weight: 700;
        }

        .hero .cta-btn {
          padding: 12px 24px;
          background-color: #ff6600;
          color: white;
          text-decoration: none;
          font-size: 1.2rem;
          border-radius: 5px;
          margin-top: 20px;
        }

        .categories {
          text-align: center;
          padding: 50px 0;
        }

        .categories h2 {
          font-size: 2rem;
          margin-bottom: 30px;
        }

        .category-grid {
          display: grid;
          grid-template-columns: repeat(4, 1fr);
          gap: 20px;
        }

        .category img {
          width: 100%;
          border-radius: 8px;
          transition: transform 0.3s;
        }

        .category img:hover {
          transform: scale(1.05);
        }

        footer {
          background-color: #333;
          color: white;
          text-align: center;
          padding: 20px;
        }

        @media (max-width: 768px) {
          .navbar .nav-links {
            display: none;
          }

          .hamburger-menu {
            display: block;
            font-size: 2rem;
            cursor: pointer;
          }

          .category-grid {
            grid-template-columns: repeat(2, 1fr);
          }
        }

        @media (max-width: 480px) {
          .hero h1 {
            font-size: 2rem;
          }

          .hero .cta-btn {
            font-size: 1rem;
          }
        }
      </style>
      <script>
        // Toggle the menu on mobile
        function toggleMenu() {
          const navLinks = document.querySelector('.nav-links');
          navLinks.classList.toggle('active');
        }
      </script>
    </head>
    <body>
      <!-- Header Section -->
      <header class="header">
        <nav class="navbar">
          <div class="logo"><a href="/">Solvayla</a></div>
          <ul class="nav-links">
            <li><a href="/">Home</a></li>
            <li><a href="/shop">Shop</a></li>
            <li><a href="/about">About</a></li>
            <li><a href="/contact">Contact</a></li>
            <li><a href="/cart">Cart</a></li>
            <li><a href="/login">Login</a></li>
          </ul>
          <div class="hamburger-menu" onclick="toggleMenu()">&#9776;</div>
        </nav>
      </header>

      <!-- Hero Section -->
      <section class="hero">
        <div class="hero-content">
          <h1>Welcome to Solvayla</h1>
          <p>Shop the latest electronics, fashion, and more with amazing deals!</p>
          <a href="/shop" class="cta-btn">Shop Now</a>
        </div>
      </section>

      <!-- Featured Categories -->
      <section class="categories">
        <h2>Featured Categories</h2>
        <div class="category-grid">
          <div class="category">
            <img src="https://via.placeholder.com/300x200" alt="Electronics">
            <h3>Electronics</h3>
          </div>
          <div class="category">
            <img src="https://via.placeholder.com/300x200" alt="Fashion">
            <h3>Fashion</h3>
          </div>
          <div class="category">
            <img src="https://via.placeholder.com/300x200" alt="Home Appliances">
            <h3>Home Appliances</h3>
          </div>
          <div class="category">
            <img src="https://via.placeholder.com/300x200" alt="Toys">
            <h3>Toys</h3>
          </div>
        </div>
      </section>

      <!-- Footer Section -->
      <footer>
        <p>&copy; 2025 Solvayla. All Rights Reserved.</p>
      </footer>
    </body>
    </html>
  `);
});

// Start Server
app.listen(5000, () => {
  console.log('Server running on http://localhost:5000');
});
