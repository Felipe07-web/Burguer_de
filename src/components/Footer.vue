<template>
    <div id="footer">
      <p>Crie Seu Hambúrguer &copy; 2023</p>
    </div>
  </template>
  
  <script>
  export default {
    name: "Footer"
  }
  </script>
  
  <style scoped>
    #footer {
      height: 150px;
      background-color: var(--dark-bg);
      border-top: 4px solid var(--primary-color);
      color: var(--primary-color);
      display: flex;
      align-items: center;
      justify-content: center;
      bottom: 0;
      width: 100%;
      font-weight: 500;
    }
  </style>