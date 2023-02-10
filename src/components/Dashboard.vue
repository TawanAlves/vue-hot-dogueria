<template>
  <div>
    <Message :msg="msg" v-show="msg" />
  </div>
  <div id="hotdog-table">
    <div>
      <div id="hotdog-table-heading">
        <div class="order-id">#:</div>
        <div>Cliente:</div>
        <div>Pão:</div>
        <div>Carne:</div>
        <div>Opcionais:</div>
        <div>Ações:</div>
      </div>
    </div>
    <div id="hotdog-table-rows">
      <div class="hotdog-table-row" v-for="hot in hots" :key="hot.id">
        <div class="order-number">{{ hot.id }}</div>
        <div>{{ hot.nome }}</div>
        <div>{{ hot.pao }}</div>
        <div>{{ hot.salsicha }}</div>
        <div>
          <ul v-for="(opcional, index) in hot.opcionais" :key="index">
            <li>{{ opcional }}</li>
          </ul>
        </div>
        <div>
          <select
            name="status"
            class="status"
            @change="updateHotdog($event, hot.id)"
          >
            <option
              :value="s.tipo"
              v-for="s in status"
              :key="s.id"
              :selected="hot.status == s.tipo"
            >
              {{ s.tipo }}
            </option>
          </select>
          <button class="delete-btn" @click="deleteHotdog(hot.id)">
            Cancelar
          </button>
        </div>
      </div>
    </div>
  </div>
  <div>
    <h2>Não há pedidos no momento!</h2>
  </div>
</template>
 
 <script>
import Message from "./Message.vue";
export default {
  name: "Dashboard",
  components: {
    Message,
  },
  data() {
    return {
      hots: null,
      hot_id: null,
      status: [],
      msg: null,
    };
  },
  methods: {
    async getPedidos() {
      const req = await fetch("http://localhost:3000/hotdogs");

      const data = await req.json();

      this.hots = data;

      this.getStatus();
    },
    async getStatus() {
      const req = await fetch("http://localhost:3000/status");
      const data = await req.json();

      this.status = data;

      console.log(data);
    },
    async deleteHotdog(id) {
      const req = await fetch(`http://localhost:3000/hotdogs/${id}`, {
        method: "DElETE",
      });

      const res = await req.json();

      this.msg = `Pedido Nº ${id} deletado com sucesso`;

      // TODO: limpar mensagem
      setTimeout(() => (this.msg = ""), 3000);

      this.getPedidos();
    },
    async updateHotdog(event, id) {
      const option = event.target.value;
      const dataJson = JSON.stringify({ status: option });
      const req = await fetch(`http://localhost:3000/hotdogs/${id}`, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: dataJson,
      });
      const res = await req.json();
      console.log(res);
    },
  },
  mounted() {
    this.getPedidos();
  },
};
</script>
 
 
 
 <style scoped>
#hotdog-table {
  max-width: 1200px;
  margin: 0 auto;
}
#hotdog-table-heading,
#hotdog-table-rows,
.hotdog-table-row {
  display: flex;
  flex-wrap: wrap;
}
#hotdog-table-heading {
  font-weight: bold;
  padding: 12px;
  border-bottom: 3px solid #333;
}
.hotdog-table-row {
  width: 100%;
  padding: 12px;
  border-bottom: 1px solid #ccc;
}
#hotdog-table-heading div,
.hotdog-table-row div {
  width: 19%;
}
#hotdog-table-heading .order-id,
.hotdog-table-row .order-number {
  width: 5%;
}
select {
  padding: 12px 6px;
  margin-right: 12px;
}
.delete-btn {
  background-color: #222;
  color: #fcba03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.5s;
}

.delete-btn:hover {
  background-color: transparent;
  color: #222;
}
</style>