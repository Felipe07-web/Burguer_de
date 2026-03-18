<template>
    <div id="main-banner">
      <div class="banner-content">
        <h1>Crie Seu Hambúrguer</h1>
        <p>A melhor experiência artesanal montada por você.</p>
      </div>
    </div>
  </template>
  
  <script>
    export default {
      name: "Banner"
    }
  </script>
  
  <style scoped>
  
    #main-banner {
      background-image: linear-gradient(rgba(0,0,0,0.5), rgba(0,0,0,0.5)), url('/img/burger.jpg');
      background-position: center;
      background-size: cover;
      height: 550px;
      display: flex;
      align-items: center;
      justify-content: flex-start;
      padding-left: 10%;
    }
    
    .banner-content {
      background: rgba(40, 40, 40, 0.4);
      backdrop-filter: blur(8px);
      -webkit-backdrop-filter: blur(8px);
      border-left: 6px solid var(--primary-color);
      padding: 50px;
      border-radius: 0 16px 16px 0;
      box-shadow: 0 15px 35px rgba(0,0,0,0.3);
      max-width: 600px;
      animation: fadeIn 1s ease-out;
    }
  
    #main-banner h1 {
      color: #FFF;
      text-align: left;
      font-size: 56px;
      font-weight: 700;
      margin-bottom: 10px;
      line-height: 1.1;
      left: 0;
      transform: none;
    }
    
    #main-banner h1::after {
      display: none;
    }
    
    #main-banner p {
      color: #ddd;
      font-size: 22px;
      font-weight: 400;
    }
    
    @keyframes fadeIn {
      from { opacity: 0; transform: translateX(-50px); }
      to { opacity: 1; transform: translateX(0); }
    }
  
  </style>