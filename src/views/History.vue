<template>
  <div class="history">
    <div class="container-fluid">
        <div class="row">
            <div class="col-md-2 col-sm-4">
                <nav class="navbar navbar-light">
                  <img @click="btnStrip()" src="../assets/img/strip.png" class="img-thumbnail img-strip" alt="Menu">
                </nav>
            </div>
            <div class="col-md-8 col-sm-6">
                <div class="text-center" style="font-size: 30px; font-weight: bold;">History</div>
            </div>
        </div>
    </div>
    <div class="container-fluid">
        <div class="row">
            <div class="col-md-1 text-center">
                <div v-if="btnStripShow">
                  <nav id="side-bar" class="navbar navbar-light">
                      <router-link to="/">
                          <img src="../assets/img/fork.png" class="img-fluid" alt="Food">
                      </router-link>
                      <router-link to="/history">
                          <img src="../assets/img/clipboard.png" class="img-fluid" alt="Clipboard">
                      </router-link>
                      <div>
                        <img v-b-modal.modal-center src="../assets/img/add.png" />
                        <componentAddProduct/>
                      </div>
                  </nav>
              </div>
            </div>
            <div class="col-md-11">
                <div class="container-fluid" style="box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.25);">
                    <div class="row">
                        <div id="history-box" class="col text-justify first-box">
                            <div class="first-history-text">Today's Income</div>
                            <div class="history-number">Rp. 10.000</div>
                            <div class="history-text">+2% Yesterday</div>
                            <div id="dot">
                                <span class="bottom-dot"></span>
                                <span class="middle-dot"></span>
                                <span class="top-dot"></span>
                            </div>
                        </div>
                        <div id="history-box" class="col text-justify second-box">
                            <div class="first-history-text">Orders</div>
                            <div class="history-number">3.270</div>
                            <div class="history-text">+5% Last Week</div>
                            <div id="dot">
                                <span class="bottom-dot"></span>
                                <span class="middle-dot"></span>
                                <span class="top-dot"></span>
                            </div>
                        </div>
                        <div id="history-box" class="col text-justify third-box">
                            <div class="first-history-text">This Year's Income</div>
                            <div class="history-number">Rp. 100.000.000.000</div>
                            <div class="history-text">+10% Last Year</div>
                            <div id="dot">
                                <span class="bottom-dot"></span>
                                <span class="middle-dot"></span>
                                <span class="top-dot"></span>
                            </div>
                        </div>
                    </div>
                    <div class="row container-fluid" style="padding: 50px 0px 40px 0px">
                        <div class="col-md-12"><div id="revenue">Revenue</div>
                            <div style="width: 100%;height: auto">
                              <table class="table">
                                <thead class="thead-dark">
                                  <tr>
                                    <th scope="col">Invoice</th>
                                    <th scope="col">Cashier</th>
                                    <th scope="col">Date</th>
                                    <th scope="col">Orders</th>
                                    <th scope="col">Amount</th>
                                  </tr>
                                </thead>
                                  <tbody>
                                    <tr v-for="(element, index) in listHistory" :key="index">
                                        <td>{{element.invoice}}</td>
                                        <td>{{element.cashier}}</td>
                                        <td>{{element.date}}</td>
                                        <td>{{element.orders}}</td>
                                        <td>{{element.amount}}</td>
                                    </tr>
                                  </tbody>
                              </table>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
  </div>
</template>

<script>
import Axios from 'axios'
import componentAddProduct from '../components/AddProduct'
export default {
  data () {
    return {
      btnStripShow: false,
      listHistory: []
    }
  },
  mounted () {
    Axios.get('http://localhost:3000/history?name=&page=&limit=').then((response) => {
      this.listHistory = response.data
      console.log(response.data)
    }).catch((err) => {
      console.log(err)
    })
  },
  methods: {
    btnStrip: function () {
      this.btnStripShow = !this.btnStripShow
    }
  },
  components: {
    componentAddProduct
  }
}
</script>
<style scoped>
#side-bar img{
  margin-top: 20px;
  margin-left: 18px;
  margin-bottom: 20px;
  height: 32px;
  width: auto;
}
#history-box{
  width: auto;
  height: 170px;
  margin: 20px 40px 0px 10px;
  padding: 25px;
}
#revenue{
    font-size: 28px;
    font-weight: bold;
    padding: 10px 0px 20px 40px;
    box-shadow: 10px 10px 20px rgba(0, 0, 0, 0.25);
    border-radius: 10px;
}
#history-box.first-box{
  background: linear-gradient(278.29deg, #FBB2B4 30.05%, rgba(255, 143, 178, 0) 133.19%);
  filter: drop-shadow(10px 15px 10px rgba(255, 143, 178, 0.35));
  border-radius: 10px;
}
#history-box.second-box{
  background: linear-gradient(278.29deg, #29DFFF 30.05%, rgba(41, 223, 255, 0) 133.19%);
  filter: drop-shadow(10px 15px 10px rgba(41, 223, 255, 0.35));
  border-radius: 10px;
}
#history-box.third-box{
  background: linear-gradient(278.29deg, #AB84C8 30.05%, rgba(241, 201, 236, 0) 133.19%);
  filter: drop-shadow(10px 15px 10px rgba(241, 201, 236, 0.50));
  border-radius: 10px;
}
.img-strip{
  margin-top: 18px;
}
.first-history-text{
  font-size: 11px;
  font-weight: bold;
  padding-top: 20px;
}
.history-text{
  font-size: 11px;
  font-weight: bold;
}
.history-number{
  font-size: 25px;
  font-weight: bold;
}
.first-box #dot span{
  background: rgba(253, 211, 228, 0.3);
}
.second-box #dot span{
  background: rgba(207, 246, 253, 0.3);
}
.third-box #dot span{
  background: rgba(241, 201, 236, 0.15);
}
.bottom-dot {
  height: 70px;
  width: 70px;
  border-radius: 50%;
  right: 25px;
  top: 10px;
  position: absolute;
}
.top-dot {
  height: 70px;
  width: 70px;
  border-radius: 50%;
  right: 50px;
  top: 30px;
  position: absolute;
}
.middle-dot {
  height: 70px;
  width: 70px;
  border-radius: 50%;
  right: 75px;
  top: 50px;
  position: absolute;
}
</style>
