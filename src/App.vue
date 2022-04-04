<template>
  <div id="app">
    <div class="container">
      <h1 class="title">2nd largest of Transactions:</h1>
      <span class="sub-title">1.10 ETH</span>
      <div class="total-list">
        <div class="total">
          <div class="total-top">
            <div class="icon">
              <img src="@/assets/imgs/coin-eth.svg" alt="" />
            </div>
            <h5 class="yellow">Total transactions:</h5>
          </div>
          <strong>79</strong>
        </div>
        <div class="total">
          <div class="total-top">
            <div class="icon">
              <img src="@/assets/imgs/coin-eth.svg" alt="" />
            </div>
            <h5 class="blue">AVG of block time</h5>
          </div>
          <strong>19.455</strong>
        </div>
        <div class="total">
          <div class="total-top">
            <div class="icon">
              <img src="@/assets/imgs/coin-eth.svg" alt="" />
            </div>
            <h5 class="green">AVG of ETH/transactions</h5>
          </div>
          <strong>1.10 ETH</strong>
        </div>
      </div>

      <form class="filter" @submit="handleFilter()">
        <input
          type="text"
          placeholder="Lọc name"
          class="filter-input"
          v-model="this.name"
        />
        <input
          type="text"
          placeholder="Lọc symbol"
          class="filter-input"
          v-model="this.symbol"
        />
        <input
          type="text"
          placeholder="Lọc status"
          class="filter-input"
          v-model="this.status"
        />
        <input
          type="text"
          placeholder="Lọc totalRaise"
          class="filter-input"
          v-model="this.totalRaise"
        />
        <input
          type="text"
          placeholder="Lọc personal allocation"
          class="filter-input"
          v-model="this.personalAllocation"
        />
        <input type="submit" value="Lọc" />
      </form>
      <div class="list-card">
        <div class="card" v-for="card in this.listCard" :key="card.id">
          <div class="img-card">
            <img :src="card.coverPhoto" alt="" />
            <button>{{ card.status }}</button>
            <div class="img-coin">
              <img :src="card.photo" alt="" />
            </div>
          </div>
          <div class="card-header">
            <h2>{{ card.name }}</h2>
            <div class="icon">
              <img src="@/assets/imgs/coin-eth.svg" alt="" />
            </div>
          </div>
          <span class="sub-card">$BCMC</span>
          <div class="card-main">
            <div class="total-raise">
              <h3>Total Raise</h3>
              <strong>${{ card.totalRaise }} Max</strong>
            </div>
            <div class="allocation">
              <h3>Personal Allocation</h3>
              <strong>${{ card.personalAllocation }}</strong>
            </div>
            <p class="card-footer">IDO starts on October 14th 2021</p>
          </div>
        </div>
      </div>

      <div class="loadmore">
        <span class="btn-load-more" @click="handleLoadMore()"
          >See All Funded Projects</span
        >
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "App",
  components: {},
  created() {
    axios
      .post(this.baseURL, {
        page: 1,
        pageSize: 20,
        symbol: "",
        name: "",
        status: "SOLD_OUT",
        totalRaise: [100, 200],
        personalAllocation: [0.07, 0.08],
      })
      .then((data) => {
        this.cards = data.data.data.fundProjects;
      });
  },
  data() {
    return {
      baseURL: "http://139.99.62.190:8000/api/v1/fund_projects/filter",
      cards: [],
      length: 5,
      name: "",
      symbol: "",
      status: "",
      totalRaise: "",
      personalAllocation: "",
    };
  },
  // mounted() {
  //   window.addEventListener('scroll',()=> {
  //     let scrollTop = document.documentElement.scrollTop;
  //     let scrollHeight = document.documentElement.scrollHeight;
  //     let clientHeight = document.documentElement.clientHeight;
  //   })
  //   let vm = this;
  //   axios.post(this.baseURL, {
  //       page: 1,
  //       pageSize: 20,
  //       symbol: "",
  //       name: "",
  //       status: "SOLD_OUT",
  //       totalRaise: [100, 200],
  //       personalAllocation: [0.07, 0.08],
  //     })
  // },
  methods: {
    handleLoadMore() {
      if (this.length > this.listCard.length) return;
      this.length = this.length + 3;
    },
    handleFilter() {
      console.log(this.name);
      axios
        .post(this.baseURL, {
          name: this.name,
          symbol: this.symbol,
          status: this.status,
          totalRaise: this.totalRaise,
          personalAllocation: this.personalAllocation,
        })
        .then((data) => {
          console.log("success", data);
        });
    },
  },
  computed: {
    listCard() {
      return this.cards.slice(0, this.length);
    },
  },
};
</script>

<style scoped>
#app {
  background: linear-gradient(0deg, rgba(0, 0, 0, 0.2), rgba(0, 0, 0, 0.2)),
    #414141;
  height: auto;
}
.container {
  width: 90%;
  margin: 0 auto;
}
.title {
  text-align: center;
  color: #fff;
  font-weight: 700;
  font-size: 48px;
  line-height: 133%;

  letter-spacing: -0.005em;
}
.sub-title {
  display: block;
  text-align: center;
  color: #fff;
  font-weight: 500;
  font-size: 20px;
  line-height: 138%;
  letter-spacing: 0.0075em;
  margin-bottom: 40px;
}
.yellow {
  color: #f0d042;
}
.green {
  color: #1f8b24;
  }
.blue {
  color: #31b4d9;
}
.total-list {
  display: flex;
  justify-content: space-around;
}
.total {
  display: flex;
  flex-direction: column;
  text-align: center;
  margin-bottom: 30px;
}
.total h5 {
  font-weight: 500;
  font-size: 20px;
  line-height: 138%;
}
.total strong {
  font-weight: 700;
  font-size: 48px;
  line-height: 133%;
  letter-spacing: -0.005em;
  color: #fff;
}
.total-top {
  display: flex;
  align-items: center;
}
.total-top .icon {
  margin-right: 12px;
}
.list-card {
  margin-bottom: 30px;
  display: grid;
  grid-template-columns: auto auto auto auto;
  gap: 32px;
}
.card {
  background: #414141;
  border-radius: 12px;
  color: #fff;
  padding: 12px;
  cursor: pointer;
  box-shadow: 1px 1px 30px rgb(43, 42, 42);
}
.img-card {
  display: block;
  position: relative;
  border-radius: 10px;
  overflow: hidden;
}
.img-card button {
  background: #238fab;
  border-radius: 4px;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  padding: 4px 12px;
  border: none;
  outline: none;
  color: #fff;
  font-weight: 700;
  font-size: 16px;
  line-height: 150%;
  position: absolute;
  top: 16px;
  right: 16px;
  cursor: pointer;
  transition: all 0.2s ease-in-out;
}
.img-card button:hover {
  background: #1a6679;
}
.img-coin {
  position: absolute;
  bottom: 16px;
  left: 16px;
  width: 50px;
  height: 50px;
  background: rgba(255, 255, 255, 0.75);
  border-radius: 12px;
  overflow: hidden;
}
.img-card img {
  width: 100%;
  object-fit: cover;
}
.card-header {
  width: 100%;
  display: flex;
  justify-content: space-between;
  /* align-items: center; */
}
.card-header .icon {
  padding: 28.22px 0;
}
.card h2 {
  font-size: 34px;
  line-height: 130%;
  max-width: 60%;
}
.card .sub-card {
  color: #c0c0c0;
  font-weight: 700;
  font-size: 16px;
  line-height: 150%;
  letter-spacing: 0.005em;
}
.allocation h3,
.total-raise h3 {
  font-weight: normal;
}
.total-raise,
.allocation {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.allocation > strong,
.total-raise > strong {
  font-weight: 500;
  font-size: 20px;
  line-height: 138%;
  text-align: center;
  letter-spacing: 0.0075em;
}
.card-footer {
  display: flex;
  justify-content: flex-end;
}

/* ===============loadmore */
.loadmore {
  display: flex;
  justify-content: center;
}
.btn-load-more {
  display: inline-block;
  margin: 0 auto;
  color: #f4f5f6;
  border: 2px solid #fff;
  border-radius: 10px;
  padding: 16px 25px;
  background: linear-gradient(0deg, rgba(0, 0, 0, 0.2), rgba(0, 0, 0, 0.2)),
    #414141;
  cursor: pointer;
  font-weight: 800;
  transition: all 0.2s linear;
}
.btn-load-more:hover {
  background: #fff;
  color: black;
}
.filter-input {
  background: #414141;
  color: #fff;
  outline: none;
  border: none;
  border-radius: 10px;
  padding: 14px 20px;
  width: 300px;
  font-size: 20px;
  /* display: block; */
  margin-bottom: 10px;
  margin-right: 10px;
  width: 14%;
}

.filter {
  margin-bottom: 80px;
  color: #fff;
  display: flex;
  align-items: center;
  height: auto;
}
.filter input[type="submit"] {
  background: #1a6679;
  color: #fff;
  outline: none;
  border: none;
  padding: 14px 20px;
  border-radius: 10px;
  margin-top: -10px;
  cursor: pointer;
  transition: all 0.1s linear;
}
.filter input[type="submit"]:hover {
  background: #20a8ca;
}
/* ===============responvie============ */

/* Small devices (portrait tablets and large phones, 600px and up) */
@media only screen and (max-width: 600px) {
  .total-list {
    flex-direction: column;
    align-items: center;
  }
  .filter-input {
    display: block;
    width: 100%;
    /* margin: 0 10px; */
  }
  .filter input[type="submit"] {
    margin-top: 10px;
    width: 100%;
  }
  .filter {
    flex-direction: column;
  }
}

/* Medium devices (landscape tablets, 768px and up) */
@media only screen and (max-width: 480px) {
  .list-card {
    grid-template-columns: auto !important;
    align-items: center;
  }
}

/* Large devices (laptops/desktops, 992px and up) */
@media only screen and (max-width: 992px) {
  .list-card {
    grid-template-columns: auto auto;
    align-items: center;
  }
}

/* Extra large devices (large laptops and desktops, 1200px and up) */
@media only screen and (max-width: 1200px) {
}
</style>
