<template>
  <Message :msg="msg" v-show="msg" />
  <div>
    <form id="burger-form" method="POST" @submit="createBurger">
      <div class="input-container">
        <label for="nome">Nome do cliente:</label>
        <input type="text" id="nome" name="nome" v-model="nome" placeholder="Digite o seu nome">
      </div>
      <div class="input-container">
        <label for="pao">Escolha o pão:</label>
        <select name="pao" id="pao" v-model="pao">
          <option value="">Selecione o seu pão</option>
          <option v-for="pao in paes" :key="pao.id" :value="pao.nome">{{ pao.nome }}</option>
        </select>
      </div>
      <div class="input-container">
        <label for="carne">Escolha a carne do seu Burger:</label>
        <select name="carne" id="carne" v-model="carne">
          <option value="">Selecione o tipo de carne</option>
          <option v-for="carne in carnes" :key="carne.id" :value="carne.nome">{{ carne.nome }}</option>
        </select>
      </div>
      <div id="opcionais-container" class="input-container">
        <label id="opcionais-title" for="opcionais">Selecione os opcionais:</label>
        <div class="checkbox-container" v-for="opcional in opcionaisdata" :key="opcional.id">
          <input type="checkbox" name="opcionais" v-model="opcionais" :value="opcional.nome">
          <span>{{ opcional.nome }}</span>
        </div>
      </div>
      <div class="input-container">
        <input class="submit-btn" type="submit" value="Criar meu Burger!">
      </div>
    </form>
  </div>
</template>

<script>
import Message from './Message'

export default {
  name: "BurgerForm",
  data() {
    return {
      paes: null,
      carnes: null,
      opcionaisdata: null,
      nome: null,
      pao: null,
      carne: null,
      opcionais: [],
      status: "Solicitado",
      msg: null
    }
  },
  methods: {
    async getIngredientes() {
      const req = await fetch(`${process.env.VUE_APP_SUPABASE_URL}/rest/v1/ingredientes?select=*`, {
        headers: {
          apikey: process.env.VUE_APP_SUPABASE_KEY,
          Authorization: `Bearer ${process.env.VUE_APP_SUPABASE_KEY}`
        }
      })
      const data = await req.json()

      // O Supabase retorna tudo como uma lista. Vamos separar pelos tipos:
      this.paes = data.filter(item => item.tipo.toLowerCase() === 'pao')
      this.carnes = data.filter(item => item.tipo.toLowerCase() === 'carne')
      this.opcionaisdata = data.filter(item => item.tipo.toLowerCase() === 'opcional')
    },
    async createBurger(e) {

      e.preventDefault()

      const data = {
        nome: this.nome,
        carne: this.carne,
        pao: this.pao,
        opcionais: Array.from(this.opcionais),
        status: "Solicitado"
      }

      const dataJson = JSON.stringify(data)    

      // Reparou no "burguers"? Estou usando o nome exato da tabela que você criou!
      await fetch(`${process.env.VUE_APP_SUPABASE_URL}/rest/v1/burguers`, {
        method: "POST",
        headers: { 
          "Content-Type" : "application/json",
          "apikey": process.env.VUE_APP_SUPABASE_KEY,
          "Authorization": `Bearer ${process.env.VUE_APP_SUPABASE_KEY}`,
          "Prefer": "return=representation"
        },
        body: dataJson
      });

      this.msg = "Pedido realizado com sucesso!"

      // clear message
      setTimeout(() => this.msg = "", 3000)

      // limpar campos
      this.nome = ""
      this.carne = ""
      this.pao = ""
      this.opcionais = []
      
    }
  },
  mounted () {
    this.getIngredientes()
  },
  components: {
    Message
  }
}
</script>

<style scoped>
  #burger-form {
    max-width: 600px;
    margin: 0 auto;
    background-color: #fff;
    padding: 40px;
    border-radius: var(--radius);
    box-shadow: 0 10px 30px rgba(0,0,0,0.08);
  }

  .input-container {
    display: flex;
    flex-direction: column;
    margin-bottom: 25px;
  }

  label {
    font-weight: 600;
    margin-bottom: 15px;
    color: var(--dark-bg);
    padding: 5px 12px;
    border-left: 4px solid var(--primary-color);
  }

  input, select {
    padding: 12px 16px;
    width: 100%;
    border: 1px solid var(--border-color);
    border-radius: 8px;
    font-size: 16px;
    transition: all 0.3s;
    background-color: var(--light-bg);
  }
  
  input:focus, select:focus {
    outline: none;
    border-color: var(--primary-color);
    box-shadow: 0 0 0 3px rgba(252, 186, 3, 0.2);
    background-color: #fff;
  }

  #opcionais-container {
    flex-direction: row;
    flex-wrap: wrap;
    gap: 15px;
  }

  #opcionais-title {
    width: 100%;
    margin-bottom: 20px;
  }

  .checkbox-container {
    display: flex;
    align-items: center;
    width: calc(50% - 15px);
    margin-bottom: 10px;
    padding: 10px;
    border-radius: 8px;
    background-color: var(--light-bg);
    transition: background-color 0.2s;
  }
  
  .checkbox-container:hover {
    background-color: #f0f0f0;
  }

  .checkbox-container span,
  .checkbox-container input {
    width: auto;
    cursor: pointer;
  }
  
  .checkbox-container input {
    width: 18px;
    height: 18px;
    accent-color: var(--primary-color);
  }

  .checkbox-container span {
    margin-left: 10px;
    font-weight: 500;
  }

  .submit-btn {
    background-color: var(--dark-bg);
    color: var(--primary-color);
    font-weight: 700;
    border: none;
    border-radius: 8px;
    padding: 16px;
    font-size: 16px;
    width: 100%;
    cursor: pointer;
    transition: all 0.3s;
    text-transform: uppercase;
    letter-spacing: 1px;
    margin-top: 10px;
  }

  .submit-btn:hover {
    background-color: var(--primary-color);
    color: var(--dark-bg);
    transform: translateY(-2px);
    box-shadow: 0 5px 15px rgba(252, 186, 3, 0.3);
  }
</style>