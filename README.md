# 🛒 Shopping Cart App

A small Vue 3 demo project that implements a simple e-commerce shopping cart.
The app allows users to browse products, add them to the cart, update item quantities, remove products, and clear the cart. The cart state is saved in localStorage, so it persists after page reloads.

# 🚀 Features
📦 View product catalog
➕ Add products to the shopping cart
🔄 Increase/decrease product quantity
❌ Remove products or clear the entire cart
💾 Cart state persistence with localStorage
💡 Notification (modal window) on product add
🎨 Styled with SCSS
🌐 Deployed on GitHub Page


# 🛠 Technologies Used
Vue 3 — (Composition API, <script setup>)
[Vue CLI / Vite] (depending on project setup, most likely Vue CLI in your case)
SCSS — for styling
localStorage — for state persistence
GitHub Pages — for deployment

# 📂 Project Structure
src/
├─ components/
│   ├─ ProductList.vue   # Product list component
│   ├─ ProductCart.vue   # Shopping cart component
├─ App.vue               # Root component
├─ main.js               # Entry point


## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).


# 🌍Deployment

**Deployment is done via GitHub Pages using the gh-pages package.**

npm run build
npm run deploy

**The app will be available at:*
https://maxkukish8.github.io/mkulish-testwork-ui-arts/