<template>
    <div>
      <div id="nav">
        <div class="nav-links">
          <router-link to="/">Fazer Pedido</router-link>
          <router-link to="/pedidos">Pedidos</router-link>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  
  export default {
    name: "Navbar",
    props: ["logo", "alt"]
  }
  </script>
  
  <style scoped>
    #nav {
      background-color: var(--dark-bg);
      border-bottom: 4px solid var(--primary-color);
      padding: 15px 50px;
      display: flex;
      justify-content: flex-end;
      align-items: center;
      box-shadow: 0 4px 10px rgba(0,0,0,0.15);
      position: sticky;
      top: 0;
      z-index: 100;
    }
  
    .nav-links {
      display: flex;
      gap: 15px;
    }
  
    #nav a {
      color: #fff;
      text-decoration: none;
      font-weight: 500;
      padding: 10px 18px;
      border-radius: var(--radius);
      transition: all 0.3s ease;
    }
  
    #nav a:not(#logo-url):hover,
    #nav a.router-link-exact-active:not(#logo-url) {
      background-color: var(--primary-color);
      color: var(--dark-bg);
      font-weight: 600;
    }

    @media (max-width: 768px) {
      #nav {
        padding: 15px 20px;
        justify-content: center;
      }
      
      .nav-links {
        gap: 10px;
      }
      
      #nav a {
        padding: 8px 12px;
        font-size: 14px;
      }
    }
  </style>