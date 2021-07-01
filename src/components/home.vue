<template>
  <div class="container p-4 text-start">
    <h1 class="mb-3 fw-bold title">Trade Tools</h1>
    <hr />
    <div class="row mt-5">
      <div class="col-lg-4 offset-0 text-start">
        <div class="box">
          <h4 class="mb-3 fw-bold">Stoploss Calculator</h4>
          <div>
            <input
              type="number"
              class="form-control"
              v-model="price"
              placeholder="Enter Price"
            />
          </div>
          <div class="mt-3">
            Stoploss Trigger (30%) :
            <h1 class="mb-3 fw-bold">{{ stoploss }}</h1>
          </div>
        </div>
      </div>
    </div>
    <!-- Charges calculator -->
    <!-- <div class="row mt-5">
      <h1>Trade Tracker</h1>

      <div class="col-3">
        <input
          type="number"
          class="form-control"
          v-model="strike"
          placeholder="Enter Strike"
        />
      </div>
      <div class="col-3">
        <input
          type="number"
          class="form-control"
          v-model="entry"
          placeholder="Enter Entry Price"
        />
      </div>
      <div class="col-3">
        <input
          type="number"
          class="form-control"
          v-model="exit"
          placeholder="Enter Exit Price"
        />
      </div>
      <div class="col-12 mt-2">
        <button class="btn btn-primary" @click="addData">Add Data</button>
      </div>
    </div> -->
    <!-- Results -->
    <!-- <div class="row mt-5">
      <h4>Brokerage : {{ brokerage }}</h4>
      <h4>STT : {{ stt }}</h4>
      <h4>Transation Charges : {{ tc }}</h4>
      <h4>Stamp Charges : {{ stamp }}</h4>
      <h4>SEBI Charges : {{ sebi }}</h4>
      <h4>GST Charges : {{ gst }}</h4>
      <h3>Total Charges : {{ total }}</h3>
    </div> -->
    <!-- <table class="table">
      <thead>
        <tr>
          <td>Date</td>
          <td>Strike</td>
          <td>Entry</td>
          <td>Exit</td>
          <td>Total Charges</td>
          <td>Net profit / Loss</td>
        </tr>
      </thead>
      <tr v-for="item in trades" :key="item.id">
        <td>{{ item.date }}</td>
        <td>{{ item.strike }}</td>
        <td>{{ item.entry }}</td>
        <td>{{ item.exit }}</td>
        <td>{{ item.total }}</td>
        <td>{{ item.profit }}</td>
      </tr>
    </table> -->
  </div>
</template>

<script>
import moment from "moment";
// List of charges
const charge_stt = 0.05;
const charge_transaction = 0.053;
const charge_stamp = 0.03;
export default {
  data() {
    return {
      strike: null,
      brokerage: 40,
      entry: null,
      exit: null,
      price: null,
      sebi: 0,
      trades: [],
    };
  },
  methods: {
    addData() {
      let date = moment().format("DD/MM/YYYY, dddd");
      // alert(date);
      this.trades.push({
        date: date,
        strike: this.strike,
        entry: this.entry,
        exit: this.exit,
        total: this.total.toFixed(2),
        profit: this.pl.toFixed(0),
      });
    },
  },
  mounted() {},
  computed: {
    stt() {
      // STT is 0.05% on sell side multiply by lot
      let a = (this.entry * charge_stt * 25) / 100;
      return a;
    },
    tc() {
      let profit = (this.entry - this.exit) * 25;
      let a = (profit * charge_transaction * 2) / 100;
      return a;
    },
    stamp() {
      let a = (this.entry * charge_stamp) / 100;
      return a;
    },
    gst() {
      let a = (this.brokerage + this.tc) * 0.18;
      return a;
    },
    total() {
      let total = this.brokerage + this.stt + this.tc + this.stamp + this.gst;
      return total;
    },
    pl() {
      let a = (this.entry - this.exit) * 25 - this.total;
      return a;
    },
    stoploss() {
      let sl = Number(this.price) + (this.price * 30) / 100;
      return sl;
    },
  },
};
</script>

<style scoped lang="scss">
.title {
  color: #42b983;
}

.box {
  border: 1px solid rgba(66, 185, 131, 0.3);
  padding: 20px;
  border-radius: 1px;
}

input {
  font-size: 18px;
}

table thead {
  background: #ccc;
}
</style>
