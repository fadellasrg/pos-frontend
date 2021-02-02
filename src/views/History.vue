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
                            <div style="overflow: scroll; width: 100%; height: auto; padding-top: 20px">
                              <div class="row">
                                <h5 style="padding: 5px 0px 0px 40px" class="col-2">Search</h5>
                              <div style="padding: 0px 200px 20px 0px;" class="col-10">
                                <b-form-input type="text" v-model="search" @keyup="allData('')" placeholder="Invoice"></b-form-input>
                                </div>
                                </div>
                              <table class="table table-striped table-hover">
                                <thead>
                                  <tr>
                                    <th scope="col">Invoice</th>
                                    <th scope="col">Cashier</th>
                                    <th scope="col">Date</th>
                                    <th scope="col">Orders</th>
                                    <th scope="col">Amount</th>
                                    <th scope="col">Action</th>
                                  </tr>
                                </thead>
                                <tbody>
                                  <tr v-for="(element, index) in listHistory" :key="index">
                                      <td>#{{element.invoice}}</td>
                                      <td>{{element.cashier}}</td>
                                      <td>{{element.date}}</td>
                                      <td>{{element.orders}}</td>
                                      <td>{{convertToRupiah(element.amount)}}</td>
                                      <td><button id="btn-detail" type="button" class="btn" @click="btnDetail(element.invoice)" v-b-modal.modal-xl>Detail</button></td>
                                  </tr>
                                </tbody>
                              </table>
                            </div>
                            <div style="display: flex;" class="overflow-auto">
                              <b-pagination style="align-self: flex-end" @input="allData('')"
                              v-model="currentPage" pills
                              :total-rows="rowsData"
                              :per-page="limitData"
                              size="sm">
                              </b-pagination>
                            </div>
                            <b-modal ref="my-modalHistory" hide-footer id="modal-xl" size="xl" centered title="Detail History">
                              <div style="overflow: scroll; width: 100%; height: auto; padding-top: 20px">
                                <table class="table table-striped table-hover">
                                  <thead>
                                    <tr>
                                      <th scope="col">Invoice</th>
                                      <th scope="col">Cashier</th>
                                      <th scope="col">Date</th>
                                      <th scope="col">Item</th>
                                      <th scope="col">Quantity</th>
                                      <th scope="col">Price</th>
                                      <th scope="col">PPN</th>
                                      <th scope="col">Amount</th>
                                      <th scope="col">Action</th>
                                    </tr>
                                  </thead>
                                  <tbody>
                                    <tr v-for="(item, index) in listDetail" :key="index">
                                      <td>{{item.invoice}}</td>
                                      <td>{{item.cashier}}</td>
                                      <td>{{item.date}}</td>
                                      <td>{{item.name}}</td>
                                      <td>{{item.qty}}</td>
                                      <td>{{item.price}}</td>
                                      <td>{{convertToRupiah(item.price * 0.1)}}</td>
                                      <td>{{convertToRupiah((item.price * item.qty) + (item.price * 0.1))}}</td>
                                      <td>
                                        <!-- <button v-b-modal.modal-center id="btnEdit" type="button" class="btn">Edit</button> -->
                                        <button @click="deleteData(item.id)" id="btnDelete" type="button" class="btn">Delete</button>
                                      </td>
                                    </tr>
                                  </tbody>
                                </table>
                              </div>
                            </b-modal>
                            <!-- <b-modal ref="my-modalEdit" hide-footer id="modal-center" size="xl" centered title="Edit History">
                              <div class="container-fluid">
                              <div class="row">
                              <div class="col-md-6 col-4">
                                <div>Invoice</div>
                                <div>Cashier</div>
                                <div>Item</div>
                                <div>Quantity</div>
                              </div>
                              <div class="col-md-6 col-4">
                                <input v-model="listDetail.invoice" type="text" class="form-control" placeholder="Invoice">
                                <input v-model="listDetail[0].cashier" type="text" class="form-control" placeholder="Cashier">
                                <input v-model="listDetail[0].id_product" type="text" class="form-control" placeholder="Item">
                                <input v-model="listDetail[0].qty" type="text" class="form-control" placeholder="Quantity">
                              </div>
                              <router-link to="/history">
                                <button class="btn add-btn" @click="editData(id)">Update</button>
                              </router-link>
                              <router-link to="/history">
                                <button class="btn cancel-btn">Cancel</button>
                              </router-link>
                              </div>
                              </div>
                            </b-modal> -->
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
      listHistory: [],
      listDetail: {
        id: '',
        invoice: '',
        cashier: '',
        date: '',
        id_product: '',
        price: '',
        qty: ''
      },
      currentPage: 1,
      limitData: 4,
      rowsData: '',
      search: ''
    }
  },
  mounted () {
    this.allData('')
  },
  methods: {
    allData: function (sorting) {
      const search = this.search ? this.search : ''
      Axios.get(`http://localhost:3000/history?invoice=${search}&sort=${sorting}&page=${this.currentPage}&limit=${this.limitData}`).then((response) => {
        this.listHistory = response.data.data
        this.rowsData = response.data.pagination.totalData
        // this.tax = this.listHistory.forEach((el, i) => {
        // this.tax = this.listHistory[0].amount * 0.1
        // })
        // console.log(this.tax)
      }).catch((err) => {
        console.log(err)
      })
    },
    deleteData: function (id) {
      const alert = confirm('Delete this product?')
      if (alert === true) {
        Axios.delete(`http://localhost:3000/history/${id}`).then((response) => {
          console.log(response)
          this.$router.go('/history')
        }).catch((err) => {
          console.log(err)
        })
      }
    },
    btnDetail: function (invoice) {
      // console.log(invoice)
      Axios.get(`http://localhost:3000/detailHistory/${invoice}`).then((response) => {
        this.listDetail = response.data.data
      }).catch((err) => {
        console.log(err)
      })
    },
    // editData: function () {
    //   const id = this.listDetail[0].id
    //   console.log(id)
    //   Axios.patch(`http://localhost:3000/history/${id}`, this.listDetail).then((response) => {
    //     console.log(response)
    //   }).catch((err) => {
    //     console.log(err)
    //   })
    // },
    btnStrip: function () {
      this.btnStripShow = !this.btnStripShow
    },
    convertToRupiah: function (digit) {
      const str = digit.toString().split('').reverse().join('')
      let convert = ''
      let result = ''
      for (let i = 0; i < str.length; i++) {
        if (i % 3 === 0) {
          convert += str.substr(i, 3) + '.'
        }
      }
      result += 'Rp. ' + convert.split('', convert.length - 1).reverse().join('')
      // console.log(result)
      return result
      // return number.toLocaleString('id', { style: 'currency', currency: 'IDR' }).replace(',00', '').replace('Rp', 'Rp.')
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
#btn-detail{
  background-color: rgba(240, 51, 120, 0.8);
  color: rgba(255,255,255, 0.8);
  margin-top: 5px;
}
#btnDelete{
  background-color: rgba(240, 51, 120, 0.8);
  width: 75%;
  color: rgba(255,255,255, 0.8);
  margin-top: 5px;
}
#btnEdit{
  background-color: rgba(87,202,213, 0.8);
  width: 75%;
  color: rgba(255,255,255, 0.8);
  margin-top: 5px;
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
