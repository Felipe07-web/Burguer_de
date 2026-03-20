<template>
  <div>
    <Navbar :logo="logo_src" :alt="app_name" />
    <router-view></router-view>
    <Footer />
  </div>
</template>
<script>
import Navbar from './components/Navbar'
import Footer from './components/Footer'

export default {
  data: function() {
    return {
      logo_src: "/img/logo.png",
      app_name: "Crie Seu Hambúrguer"
    }
  },
  components: {
    Navbar,
    Footer
  }
}
</script>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

  :root {
    --primary-color: #fcba03;
    --primary-hover: #e0a500;
    --dark-bg: #1e1e1e;
    --light-bg: #f8f9fa;
    --text-color: #333333;
    --border-color: #e0e0e0;
    --box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
    --radius: 12px;
  }

  * {
    font-family: 'Inter', sans-serif;
    padding: 0;
    margin: 0;
    box-sizing: border-box;
  }

  body {
    background-color: var(--light-bg);
    color: var(--text-color);
  }

  .main-container {
    margin: 50px auto;
    min-height: 250px;
    max-width: 1200px;
    padding: 0 20px;
  }

  h1 {
    text-align: center;
    font-size: 38px;
    margin-bottom: 40px;
    color: var(--dark-bg);
    font-weight: 700;
    position: relative;
    display: inline-block;
    left: 50%;
    transform: translateX(-50%);
  }
  
  h1::after {
    content: '';
    position: absolute;
    bottom: -10px;
    left: 25%;
    width: 50%;
    height: 4px;
    background-color: var(--primary-color);
    border-radius: 2px;
  }

  @media (max-width: 768px) {
    .main-container {
      margin: 30px auto;
      padding: 0 15px;
    }
    
    h1 {
      font-size: 28px;
    }
  }
</style>