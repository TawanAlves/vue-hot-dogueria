<template>
  <div>
    <Message :msg="msg" v-show="msg" />
  </div>
  <div>
    <form id="hotdog-form" @submit="createHotdog" method="POST">
      <div class="input-container">
        <label for="nome">Nome do cliente:</label>
        <input
          type="text"
          id="nome"
          name="nome"
          v-model="nome"
          placeholder="Digite o seu nome"
        />
      </div>
      <div class="input-container">
        <label for="pao">Escolha o pão:</label>
        <select name="pao" id="pao" v-model="pao">
          <option value="">Selecione o seu pão</option>
          <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">
            {{ pao.tipo }}
          </option>
        </select>
      </div>
      <div class="input-container">
        <label for="salsicha">Escolha quantas Salsichas:</label>
        <select name="salsicha" id="salsicha" v-model="salsicha">
          <option value="">Selecione o número de salsicha</option>
          <option
            v-for="salsicha in salsichas"
            :key="salsicha.id"
            :value="salsicha.tipo"
          >
            {{ salsicha.tipo }}
          </option>
        </select>
      </div>

      <div id="opcionais-container" class="input-container">
        <label id="opcionais-title" for="opcionais"
          >Selecione os opcionais:</label
        >
        <div
          class="checkbox-container"
          v-for="opcional in opcionaisdata"
          :key="opcional.id"
        >
          <input
            type="checkbox"
            name="opcionais"
            v-model="opcionais"
            :value="opcional.tipo"
          />
          <span>{{ opcional.tipo }}</span>
        </div>
      </div>
      <div class="input-container">
        <input class="submit-btn" type="submit" value="Criar meu Hot-Dog!" />
      </div>
    </form>
  </div>
</template>

<script>
import Message from "./Message.vue";
export default {
  name: "HotForm",
  components: {
    Message,
  },
  data() {
    return {
      paes: null,
      salsichas: null,
      opcionaisdata: null,
      nome: null,
      pao: null,
      salsicha: null,
      opcionais: [],
      msg: null,
    };
  },
  methods: {
    async getIngredientes() {
      const req = await fetch(" http://localhost:3000/ingredientes");
      const data = await req.json();

      this.paes = data.paes;
      this.salsichas = data.salsichas;
      this.opcionaisdata = data.opcionais;
    },

    async createHotdog(e) {
      e.preventDefault();
      const data = {
        nome: this.nome,
        salsicha: this.salsicha,
        pao: this.pao,
        opcionais: Array.from(this.opcionais),
        status: "Solicitado",
      };

      const dataJson = JSON.stringify(data);

      const req = await fetch(" http://localhost:3000/hotdogs", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: dataJson,
      });

      const res = await req.json();

      this.msg = `Pedido Nº ${res.id} realizado com sucesso`;

      setTimeout(() => (this.msg = ""), 3000);

      this.nome = "";
      this.salsicha = "";
      this.pao = "";
      this.opcionais = "";
    },
  },
  mounted() {
    this.getIngredientes();
  },
};
</script>


<style scoped>
#hotdog-form {
  max-width: 400px;
  margin: 0 auto;
}
.input-container {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;
}
label {
  font-weight: bold;
  margin-bottom: 15px;
  color: #222;
  padding: 5px 10px;
  border-left: 4px solid #fc6703;
}
input,
select {
  padding: 5px 10px;
  width: 300px;
}
#opcionais-container {
  flex-direction: row;
  flex-wrap: wrap;
}
#opcionais-title {
  width: 100%;
}
.checkbox-container {
  display: flex;
  align-items: flex-start;
  width: 50%;
  margin-bottom: 20px;
}
.checkbox-container span,
.checkbox-container input {
  width: auto;
}
.checkbox-container span {
  margin-left: 6px;
  font-weight: bold;
}
.submit-btn {
  background-color: #222;
  color: #fc6703;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.5s;
}
.submit-btn:hover {
  background-color: transparent;
  color: #222;
}
</style>