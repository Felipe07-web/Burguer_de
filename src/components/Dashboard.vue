<template>
    <div id="burger-table" v-if="burgers">
      <div>
        <div id="burger-table-heading">
          <div class="order-id">#:</div>
          <div>Cliente:</div>
          <div>Pão:</div>
          <div>Carne:</div>
          <div>Opcionais:</div>
          <div>Ações:</div>
        </div>
      </div>
      <div id="burger-table-rows">
        <div class="burger-table-row" v-for="burger in burgers" :key="burger.id">
          <div class="order-number">{{ burger.id }}</div>
          <div>{{ burger.nome }}</div>
          <div>{{ burger.pao }}</div>
          <div>{{ burger.carne }}</div>
          <div>
            <ul>
              <li v-for="(opcional, index) in burger.opcionais" :key="index">{{ opcional }}</li>
            </ul>
          </div>
          <div>
            <select name="status" class="status" @change="updateBurger($event, burger.id)">
              <option :value="s.tipo" v-for="s in status" :key="s.id" :selected="burger.status == s.tipo">
                {{ s.tipo }}
              </option>
            </select>
            <button class="delete-btn" @click="deleteBurger(burger.id)">Cancelar</button>
          </div>
        </div>
      </div>
    </div>
    <div v-else>
      <h2>Não há pedidos no momento!</h2>
    </div>
  </template>
  <script>
    export default {
      name: "Dashboard",
      data() {
        return {
          burgers: null,
          burger_id: null,
          status: []
        }
      },
      methods: {
        async getPedidos() {
          const req = await fetch(`${process.env.VUE_APP_SUPABASE_URL}/rest/v1/burguers?select=*`, {
            headers: {
              apikey: process.env.VUE_APP_SUPABASE_KEY,
              Authorization: `Bearer ${process.env.VUE_APP_SUPABASE_KEY}`
            }
          })
  
          const data = await req.json()
  
          this.burgers = data.map(burger => {
            // Se o Supabase retornou o array como String, converte de volta para um Array
            if (typeof burger.opcionais === 'string') {
              try { burger.opcionais = JSON.parse(burger.opcionais); } 
              catch(e) { burger.opcionais = []; }
            }
            return burger;
          })
  
          // Resgata os status de pedidos
          this.getStatus()
  
        },
        async getStatus() {
  
          const req = await fetch(`${process.env.VUE_APP_SUPABASE_URL}/rest/v1/status?select=*`, {
            headers: {
              apikey: process.env.VUE_APP_SUPABASE_KEY,
              Authorization: `Bearer ${process.env.VUE_APP_SUPABASE_KEY}`
            }
          })
  
          const data = await req.json()
  
          this.status = data
  
        },
        async deleteBurger(id) {
  
          await fetch(`${process.env.VUE_APP_SUPABASE_URL}/rest/v1/burguers?id=eq.${id}`, {
            method: "DELETE",
            headers: {
              apikey: process.env.VUE_APP_SUPABASE_KEY,
              Authorization: `Bearer ${process.env.VUE_APP_SUPABASE_KEY}`
            }
          });
  
          this.getPedidos()
  
        },
        async updateBurger(event, id) {
  
          const option = event.target.value;
  
          const dataJson = JSON.stringify({status: option});
  
          await fetch(`${process.env.VUE_APP_SUPABASE_URL}/rest/v1/burguers?id=eq.${id}`, {
            method: "PATCH",
            headers: { 
              "Content-Type" : "application/json",
              "apikey": process.env.VUE_APP_SUPABASE_KEY,
              "Authorization": `Bearer ${process.env.VUE_APP_SUPABASE_KEY}`
            },
            body: dataJson
          });
  
        }
      },
      mounted () {
        this.getPedidos()
      }
    }
  </script>
  
  <style scoped>
    #burger-table {
      max-width: 1200px;
      margin: 0 auto;
    }
  
    #burger-table-heading,
    #burger-table-rows,
    .burger-table-row {
      display: flex;
      flex-wrap: wrap;
    }
  
    #burger-table-heading {
      font-weight: 700;
      padding: 16px;
      border-bottom: 3px solid var(--dark-bg);
      color: var(--dark-bg);
      margin-bottom: 15px;
    }
  
    .burger-table-row {
      width: 100%;
      padding: 16px;
      border: 1px solid var(--border-color);
      background-color: #fff;
      margin-bottom: 15px;
      border-radius: var(--radius);
      box-shadow: var(--box-shadow);
      align-items: center;
      transition: all 0.3s;
    }
    
    .burger-table-row:hover {
      transform: translateY(-3px);
      box-shadow: 0 8px 15px rgba(0,0,0,0.08);
      border-color: var(--primary-color);
    }
  
    #burger-table-heading div,
    .burger-table-row div {
      width: 19%;
    }
    
    .burger-table-row ul {
      padding-left: 20px;
      color: #555;
    }
  
    #burger-table-heading .order-id,
    .burger-table-row .order-number {
      width: 5%;
      font-weight: 700;
      color: var(--dark-bg);
      font-size: 18px;
    }
  
    select {
      padding: 10px 12px;
      margin-right: 12px;
      margin-bottom: 12px;
      border: 1px solid var(--border-color);
      border-radius: 6px;
      background-color: var(--light-bg);
      cursor: pointer;
      font-weight: 500;
      transition: all 0.3s;
      width: 140px;
    }
    
    select:focus {
      outline: none;
      border-color: var(--primary-color);
      box-shadow: 0 0 0 2px rgba(252, 186, 3, 0.2);
    }
  
    .delete-btn {
      background-color: var(--dark-bg);
      color: var(--primary-color);
      font-weight: 600;
      border: none;
      border-radius: 6px;
      padding: 10px 14px;
      font-size: 14px;
      cursor: pointer;
      transition: all 0.3s;
      width: 140px;
    }
    
    .delete-btn:hover {
      background-color: #e63946; 
      color: #fff;
    }
    
    @media (max-width: 768px) {
      #burger-table-heading {
        display: none;
      }
      
      .burger-table-row {
        flex-direction: column;
        align-items: flex-start;
        gap: 10px;
      }
      
      #burger-table-heading div,
      .burger-table-row div {
        width: 100%;
      }
      
      .burger-table-row .order-number {
        width: 100%;
        margin-bottom: 10px;
        border-bottom: 1px solid var(--border-color);
        padding-bottom: 10px;
      }
      
      .burger-table-row div:nth-child(2)::before {
        content: "Cliente: ";
        font-weight: bold;
        color: var(--dark-bg);
      }
      
      .burger-table-row div:nth-child(3)::before {
        content: "Pão: ";
        font-weight: bold;
        color: var(--dark-bg);
      }
      
      .burger-table-row div:nth-child(4)::before {
        content: "Carne: ";
        font-weight: bold;
        color: var(--dark-bg);
      }
      
      .burger-table-row div:nth-child(5)::before {
        content: "Opcionais: ";
        font-weight: bold;
        color: var(--dark-bg);
        display: block;
        margin-bottom: 5px;
      }
      
      .burger-table-row ul {
        padding-left: 0;
        list-style-position: inside;
        margin-top: 5px;
      }
      
      .burger-table-row div:last-child {
        display: flex;
        flex-direction: column;
        gap: 10px;
        margin-top: 15px;
      }
      
      select {
        width: 100%;
        margin-right: 0;
        margin-bottom: 0;
      }

      .delete-btn {
        width: 100%;
      }
    }
  </style>