<template>
  <div class="burger-table">
    <MessageComponent :msg="msg" v-show="msg"/>
    <div>
      <div id="burger-table-heading">
        <div class="order-id">#:</div>
        <div>Client:</div>
        <div>Bun:</div>
        <div>Meat:</div>
        <div>Optionals:</div>
        <div>Actions:</div>
      </div>
    </div>
    <div id="burger-table-rows">
      <div class="burger-table-row" v-for="order in orders" :key="order.id">
        <div class="order-number">{{order.id}}</div>
        <div>{{order.nome}}</div>
        <div>{{order.pao}}</div>
        <div>{{order.carne}}</div>
        <div>
          <ul v-for="(optional, index) in order.opcionais" :key="index">
            <li>{{optional}}</li>
          </ul>
        </div>
        <div>
          <select name="status" class="status" @change="alterStatus($event, order.id)">
            <option value="">Select</option>
            <option
              v-for="stat in status"
              :key="stat.id"
              :value="stat.tipo"
              :selected="order.status === stat.tipo"
            >
              {{stat.tipo}}
            </option>
          </select>
          <button class="delete-btn" @click="deleteOrder(order.id)">Cancel</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import MessageComponent from './Message.vue';

export default {
  name: 'DashboardComponent',
  data() {
    return {
      orders: null,
      order_id: null,
      status: [],
      msg: '',
    };
  },
  components: {
    MessageComponent,
  },
  methods: {
    async getList() {
      const getBurgers = await fetch('http://localhost:3000/burgers');
      const data = await getBurgers.json();
      this.orders = data;
      this.getStatus();
    },
    async getStatus() {
      const getStat = await fetch('http://localhost:3000/status');
      const dataStatus = await getStat.json();
      this.status = dataStatus;
    },
    async alterStatus(event, id) {
      const option = event.target.value;
      const dataJson = JSON.stringify({ status: option });
      const req = await fetch(`http://localhost:3000/burgers/${id}`, {
        method: 'PATCH',
        headers: { 'Content-Type': 'application/json' },
        body: dataJson,
      });
      const res = await req.json();
      console.log('res', res);

      this.msg = `The order n°${res.id} was updated to ${res.status}!`;
      setTimeout(() => {
        this.msg = '';
      }, 3000);
    },

    async deleteOrder(id) {
      const req = await fetch(`http://localhost:3000/burgers/${id}`, {
        method: 'DELETE',
      });
      const res = await req.json();
      console.log('deletar', res);

      this.getList(); // atualização sistema forçada backend renderizar lista sem item
      this.msg = 'Order deleted successfully';
      setTimeout(() => {
        this.msg = '';
      }, 3000);
    },
  },
  mounted() {
    this.getList();
  },
};
</script>

<style scoped>
#burger-table{
  max-width: 1200px;
  margin: 0 auto;
}
#burger-table-heading,
#burger-table-rows,
.burger-table-row {
  display: flex;
  flex-wrap: wrap;
}
#burger-table-heading{
  font-weight: bold;
  padding: 12px;
  border-bottom: 3px solid #333;
}
#burger-table-heading div,
.burger-table-row div{
  width: 19%;
}
.burger-table-row {
  width: 100%;
  padding: 12px;
  border-bottom: 1px solid #ccc;
}

#burger-table-heading .order-id,
.burger-table-row .order-number {
  width: 5%;
}

select {
  padding: 12px 6px;
  margin-right: 12px;
}

.delete-btn{
  background-color: #222;
  color: #FCBA03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  cursor: pointer;
  transition: .5s;
}
.delete-btn:hover{
  background-color: transparent;
  color: #222;
}
</style>
