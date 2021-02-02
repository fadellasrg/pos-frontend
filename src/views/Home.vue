<template>
  <div class="container-fluid">
      <div class="row">
          <div class="col-md-8">
              <nav class="navbar navbar-light">
                  <img @click="btnStrip()" src="../assets/img/strip.png" class="img-thumbnail" alt="Menu">
                  <div v-if="btnForkShow">
                    <b-dropdown style="background-color: rgba(242,79,138, 0.8);" right text="Category">
                      <b-dropdown-item @click="sortData('')">All Category</b-dropdown-item>
                      <b-dropdown-item @click="sortData('Dessert')">Dessert</b-dropdown-item>
                      <b-dropdown-item @click="sortData('Drink')">Drink</b-dropdown-item>
                      <b-dropdown-item @click="sortData('Food')">Food</b-dropdown-item>
                    </b-dropdown>
                  </div>
                  <div id="font-header" class="text-center">Food Items</div>
                  <div v-if="btnSearchShow">
                      <b-form-input type="text" v-model="search" @keyup="paginationProduct('')" placeholder="Search menu"></b-form-input>
                  </div>
                  <img v-on:click="btnSearch()" src="../assets/img/search.png" class="img-thumbnail" alt="Search">
              </nav>
          </div>
          <div id="font-header" class="col-md-4 text-center" style="box-shadow: 0px 4px 5px rgba(0, 0, 0, 0.25);">Cart
            <a href="#hrefCart">
              <span class="badge rounded-pill text-white">{{dataCart.length}}</span>
            </a>
          </div>
      </div>

    <div class="container-fluid">
          <div class="row">
              <div class="col-md-1 text-center">
                <div v-if="btnStripShow">
                  <nav id="side-bar" class="navbar navbar-light">
                      <router-link to="/">
                          <img @click="btnFork" src="../assets/img/fork.png" class="img-fluid" alt="Food">
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
              <div class="product col-md-7 col-12 text-center">
                <div class="container-fluid">
                  <div class="row">
                    <div class="col-lg-12 col-12">
                      <div v-if="btnForkShow">
                        <div class="row">
                          <div class="col-5 btn-group" role="group">
                            <button @click="paginationProduct('asc')" type="button" class="btn btn-secondary">Lower Price</button>
                            <button @click="paginationProduct('desc')" type="button" class="btn btn-secondary">Higher Price</button>
                          </div>
                        </div>
                      </div>
                    </div>
                    <div class="col-lg-4 col-md-6 col-6" v-for="(element, index) in listProducts" v-bind:key="index">
                      <componentProducts :element = "element" v-on:actionCart="actChild" />
                    </div>
                    <div style="display: flex;" class="overflow-auto">
                      <b-pagination style="align-self: flex-end" @input="paginationProduct('')"
                      v-model="currentPage" pills
                      :total-rows="rowsData"
                      :per-page="limitData"
                      size="sm">
                      </b-pagination>
                    </div>
                  </div>
                </div>
              </div>
              <div id="hrefCart" class="col-md-4 col-sm-12 text-center" style="min-height: 100vh;">
                <div class="container-fluid">
                  <div style="display: flex" class="row cart">
                    <div v-for="(item, index) in dataCart" :key="index">
                        <div class="row cart">
                          <div class="col-md-4 col-sm-4 col-4">
                            <img :src="item.image" :alt="item.category" class="figure-img img-fluid rounded">
                          </div>
                          <div class="col-md-4 col-sm-4 col-4">{{item.name}}
                          <div class="btn-group" role="group">
                              <button @click="btnMin(item.id_product)" type="button" class="btn btn-outline-primary">-</button>
                              <button type="button" class="btn btn-outline-secondary" disabled>{{item.qty}}</button>
                              <button @click="btnPlus(item.id_product)" type="button" class="btn btn-outline-primary">+</button>
                            </div>
                          </div>
                          <div class="col-md-4 col-sm-4 col-4 price">{{ convertToRupiah (item.totalPrice) }}
                            <button id="btnDelete" @click="deleteCart(item.id_product)">Delete</button>
                        </div>
                      </div>
                    </div>
                    <div v-if="dataCart.length < 1" id="resize" class="col-md-12">
                      <img src="../assets/img/empty_cart.png" alt="Empty Cart">
                      <div style="padding: 0px 0px 0px 0px; font-weight: bold; font-size: 22px;">Your cart is empty</div>
                      <div style="padding: 0px 0px 20px 0px; color: rgba(190, 195, 202, 0.911); font-size: 14px;">Please add some items from the menu</div>
                      </div>
                    <footer style="align-self: flex-end">
                      <div v-if="dataCart.length > 0" class="col-md-12">Total : {{this.convertToRupiah(total)}}</div>
                      <div v-if="dataCart.length > 0" class="col-md-12">*Belum termasuk PPN</div>
                    </footer>
                  </div>
                    <button v-if="dataCart.length > 0" v-b-modal.my-modal v-on:click="checkout()" id="btn-checkoutPink" class="btn" type="button">Checkout
                      <!-- <componentCart/> -->
                      <b-modal ref="my-modal" hide-footer id="my-modal" centered title="Checkout">
                      <div class="row">
                        <div style="padding-bottom: 40px;" class="col-md-6">
                          <!-- <div v-for="(item, index) in dataCart" :key="index"> -->
                            <div style="font-size: 12px;">Cashier: Cashier 26</div>
                          <!-- </div> -->
                        </div>
                        <!-- <div v-for="(item, index) in dataCart" :key="index" class="col-md-6 text-right"> -->
                          <div class="col-md-6 text-right">
                            <div>Receipt no: #10932</div>
                          </div>
                        <!-- </div> -->
                      </div>
                      <div v-for="(item, index) in dataCart" :key="index" class="row">
                        <div class="col-md-6 col-sm-8 col-8 pop-menus">
                            <div>{{item.name}} {{item.qty}}x</div>
                        </div>
                        <div class="col-md-6 col-sm-4 col-4 text-right pop-price">
                            <div>{{convertToRupiah(item.price)}}</div>
                        </div>
                      </div>
                      <div class="row">
                        <div class="col-md-6 col-sm-8 col-8 pop-menus">
                            <div>PPN 10%</div>
                        </div>
                        <div class="col-md-6 col-sm-4 col-4 text-right pop-price">
                            <div>{{convertToRupiah(total * 0.1)}}</div>
                        </div>
                      </div>
                      <div class="row">
                      <div class="col-md-12 text-right">
                          <div style="padding-top: 20px; font-weight: bold">Total: {{convertToRupiah(total + (total * 0.1))}}</div>
                      </div>
                  </div>
                  <div class="row">
                      <div class="col-md-12">
                          <div>Payment: Cash</div>
                      </div>
                  </div>
                  <div id="bottom-btn-checkout" class="container-fluid text-center">
                      <div class="row">
                          <div class="col-md-12">
                              <button id="pop-pink" class="btn" type="button" v-on:click="hideModal()">Print</button>
                          </div>
                          <div class="col-md-12">Or</div>
                          <div class="col-md-12">
                              <button id="pop-blue" class="btn" type="button" v-on:click="hideModal()">Send Email</button>
                          </div>
                      </div>
                  </div>
                    </b-modal>
                    </button>
                    <button v-if="dataCart.length > 0" v-on:click="cartCancel()" id="btn-checkoutBlue" class="btn" type="button">Cancel</button>
                  </div>
            </div>
          </div>
    </div>
  </div>
</template>

<script>
import Axios from 'axios'
import componentProducts from '../components/Product'
import componentAddProduct from '../components/AddProduct'
// import componentCart from '../components/Cart'
export default {
  name: 'Product',
  data () {
    return {
      listProducts: [],
      dataCart: [],
      btnSearchShow: false, // set default (not showing)
      btnStripShow: false,
      btnForkShow: false,
      total: 0,
      search: '',
      currentPage: 1,
      limitData: 6,
      rowsData: '' // 9 total data (before limit)
    }
  },
  mounted () {
    this.paginationProduct('')
  },
  components: {
    componentProducts,
    componentAddProduct
  },
  methods: {
    paginationProduct: function (sorting) {
      const search = this.search ? this.search : ''
      // const sorting = sorting ? sorting : ''
      Axios.get(`http://localhost:3000/products?name_product=${search}&param=price&sort=${sorting}&page=${this.currentPage}&limit=${this.limitData}`).then((response) => {
        this.listProducts = response.data.data
        this.rowsData = response.data.pagination.totalData
        // console.log(response.data.pagination.totalData)
      }).catch((err) => {
        console.log(err)
      })
    },
    sortData: function (sort) {
      Axios.get('http://localhost:3000/productsCategory?name_category=' + sort + '&param=&sort=ASC&page=1&limit=100').then((response) => {
        this.listProducts = response.data
      }).catch((err) => {
        console.log(err)
      })
    },
    setTotal: function () {
      this.total = 0
      this.dataCart.forEach((el) => {
        // el.price = el.price.replace(/[\W]/g, '')
        // el.price = el.price.replace('Rp', '')
        this.total = this.total + (el.price * el.qty)
      })
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
      return result
    },
    actChild: function (element) {
      const checkProduct = this.dataCart.filter((item) => {
        return item.id_product === element.id_product
      })
      if (checkProduct.length >= 1) {
        // alert('produk sudah adaaa')
        this.dataCart.forEach((i) => {
          if (i.id_product === element.id_product) {
            i.qty += 1
          }
        })
      } else {
        const newData = {
          invoice: '10932',
          cashier: 'Cashier 26',
          id_product: element.id_product,
          qty: 1, // default 1
          name: element.name,
          price: element.price,
          image: element.image,
          totalPrice: 1 * element.price
        }
        this.dataCart = [...this.dataCart, newData]
      }
      this.setTotal()
    },
    btnMin: function (idProduct) {
      this.dataCart.map((item) => {
        if (item.id_product === idProduct) {
          item.qty -= 1
          item.totalPrice = item.qty * item.price
          if (item.qty === 0) {
            const alert = confirm('Delete this product?')
            if (alert === true) {
              const newCart = this.dataCart.filter((item) => {
                return item.id_product !== idProduct
              })
              this.dataCart = newCart
            } else {
              item.qty = 1
              item.totalPrice = item.qty * item.price
            }
          }
        }
      })
      this.setTotal()
    },
    btnPlus: function (idProduct) {
      this.dataCart.map((item) => {
        if (item.id_product === idProduct) {
          item.qty += 1
          item.totalPrice = item.qty * item.price
        }
      })
      this.setTotal()
    },
    checkout: function () {
      Axios.post('http://localhost:3000/history', this.dataCart).then((response) => {
        // console.log(response)
      }).catch((err) => {
        console.log(err)
      })
    },
    btnSearch: function () {
      // alert('123')
      this.btnSearchShow = !this.btnSearchShow
    },
    btnStrip: function () {
      this.btnStripShow = !this.btnStripShow
    },
    btnFork: function () {
      this.btnForkShow = !this.btnForkShow
    },
    deleteCart: function (idProduct) {
      // console.log(idProduct)
      const newCart = this.dataCart.filter((item) => {
        return item.id_product !== idProduct
      })
      // console.log(newCart)
      this.dataCart = newCart
      this.setTotal()
    },
    cartCancel: function () {
      this.$router.go('/')
      // this.$router.push({ path: '/' })
    },
    hideModal: function () {
      this.$refs['my-modal'].hide()
      this.$router.go('/')
    }
  }
}
</script>
<style>
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@500&display=swap');
#font-header{
  font-size: 30px;
  font-weight: bold;
}
#side-bar img{
  margin-top: 2px;
  margin-left: 5px;
  margin-bottom: 35px;
  height: 32px;
  width: auto;
}
#product{
  padding: 20px
}
#product img{
  height: 130px;
  width: 170px;
}
#resize img{
  padding: 40px 0px 0px 0px;
  height: 180px;
  width: auto;
}
div{
  font-family: 'Noto Sans JP', sans-serif;
}
.row .text-center{
  padding: 20px 0px 20px 0px;
}
.product.col-md-7{
  background-color: rgba(190, 195, 202, 0.3);
  box-shadow: 0px 4px 2px rgba(0, 0, 0, 0.25);
  padding-left: 15px;
}
.price{
  font-weight: bold;
}
.rounded-pill{
  background-color: rgba(16, 220, 235, 0.911);
}
/* .itemTitle{
  font-weight: bold;
  padding-bottom: 20px;
  font-size: 20px;
  right: 0;
  top: 0;
} */
/* .total{
  position: absolute;
  right: 0px;
  left: 8px;
  bottom: 70px;
} */
#btn-checkoutPink{
  /* position: absolute; */
  position: relative;
  right: 0;
  left: 8px;
  bottom: 6%;
  border-radius: 0px;
  background-color: rgba(242,79,138, 0.8);
  width: 100%;
  color: rgba(255,255,255, 0.8);
  margin-top: 10px;
}
#btn-checkoutBlue{
  /* position: absolute; */
  position: relative;
  right: 0;
  left: 8px;
  bottom: 2%;
  border-radius: 0px;
  background-color: rgba(87,202,213, 0.8);
  width: 100%;
  color: rgba(255,255,255, 0.8);
  margin-top: 4px;
}
.cart img{
  padding-left: 10px;
  padding-bottom: 10px;
  height: 100px;
  width: 120px;
}
.cart div{
  font-weight: bold;
  font-size: 16px;
}
.cart .price{
  font-size: 15px;
  padding: 40px 0px 0px 50px;
}
.cart .btn-group button{
    font-weight: bold;
    background: rgba(130, 222, 58, 0.2);
    border: 1px solid rgba(130, 222, 58, 0.8);
    color: rgba(130,222,58,1);
    box-sizing: border-box;
    padding: 4px 9px 4px 9px;
    margin-top: 30px;
    margin-bottom: 20px;
}
.cart .btn-group .btn-outline-secondary{
    background-color: rgba(255,255,255, 0.8);
}
#btnDelete{
  background-color: rgba(240, 51, 120, 0.8);
  width: 80%;
  color: rgba(255,255,255, 0.8);
  margin: 0;
  margin-top: 5px;
  border-radius: 6px;
}
#pop-pink{
    background-color: rgba(242,79,138, 0.8);
    width: 100%;
    color: rgba(255,255,255, 0.8);
    margin-top: 20px;
    border-radius: 6px;
}
#pop-blue{
    background-color: rgba(87,202,213, 0.8);
    width: 100%;
    color: rgba(255,255,255, 0.8);
    margin: 0;
    border-radius: 6px;
}
.pop-menus div{
    padding-bottom: 8px;
    font-weight: bold;
}
.pop-price div{
    padding-bottom: 8px;
    font-weight: bold;
}
footer div{
  position: relative;
  bottom: 10%;
  font-weight: bold;
  font-size: 15px;
}
/* @media (min-width: 300px) and (max-width: 767px) {
  .product.col-7{
  }
} */
</style>
