<template>
  <div class="container-fluid">
      <div class="row">
          <div class="col-md-8">
              <nav class="navbar navbar-light">
                  <img @click="btnStrip()" src="../assets/img/strip.png" class="img-thumbnail" alt="Menu">
                  <div>
                      <!-- <b-dropdown style="background-color: rgba(242,79,138, 0.8);" right text="Sort">
                        <b-dropdown-item v-model="sort" @click="sortData()">Dessert</b-dropdown-item>
                        <b-dropdown-item v-model="sort" @click="sortData()">Drink</b-dropdown-item>
                        <b-dropdown-item v-model="sort" @click="sortData()">Food</b-dropdown-item>
                      </b-dropdown> -->
                      <div v-if="btnForkShow">
                      <b-form-input type="text" v-model="sort" @keyup="sortData()" placeholder="Sorting menu"></b-form-input>
                      </div>
                  </div>
                  <div id="font-header" class="text-center">Food Items</div>
                  <div v-if="btnSearchShow">
                      <b-form-input type="text" v-model="search" @keyup="findData()" placeholder="Search menu"></b-form-input>
                  </div>
                  <img v-on:click="btnSearch()" src="../assets/img/search.png" class="img-thumbnail" alt="Search">
              </nav>
          </div>
          <div id="font-header" class="col-md-4 text-center" style="box-shadow: 0px 4px 5px rgba(0, 0, 0, 0.25);">Cart
              <span class="badge rounded-pill text-white">{{dataCart.length}}</span>
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
                    <div v-for="(element, index) in listProducts" v-bind:key="index">
                      <componentProducts :element = "element" v-on:actionCart="actChild" />
                    </div>
                  </div>
                </div>
              </div>
              <div class="col-md-4 col-sm-12 text-center" style="min-height: 120vh;">
                <div class="container-fluid">
                  <div class="row cart">
                    <div v-for="(item, index) in dataCart" :key="index">
                        <div class="row cart">
                          <div class="col-md-4 col-sm-6">
                            <img :src="item.image" :alt="item.category" class="figure-img img-fluid rounded">
                          </div>
                          <div class="col-md-4 col-sm-2">{{item.name}}
                          <div class="btn-group" role="group">
                              <button type="button" class="btn btn-outline-primary">-</button>
                              <button type="button" class="btn btn-outline-secondary" disabled>{{item.qty}}</button>
                              <button type="button" class="btn btn-outline-primary">+</button>
                            </div>
                          </div>
                          <div class="col-md-4 col-sm-6 price">{{item.price}}
                            <button id="btnDelete" @click="deleteCart(item.id_product)">Delete</button>
                        </div>
                      </div>
                    </div>
                    <footer>
                      <div class="col-md-12 total">Total : {{this.convertToRupiah(total)}}</div>
                      <div class="col-md-12 total">*Belum termasuk PPN</div>
                    </footer>
                    <button v-b-modal.my-modal v-on:click="checkout()" id="btn-checkoutPink" type="button">Checkout
                      <!-- <componentCheckout/> -->
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
                            <div>{{item.name}}</div>
                        </div>
                        <div class="col-md-6 col-sm-4 col-4 text-right pop-price">
                            <div>{{item.price}}</div>
                        </div>
                      </div>
                      <div class="row">
                        <div class="col-md-6 col-sm-8 col-8 pop-menus">
                            <div>PPN 10%</div>
                        </div>
                        <div class="col-md-6 col-sm-4 col-4 text-right pop-price">
                            <div>{{this.convertToRupiah(total * 0.1)}}</div>
                        </div>
                      </div>
                      <div class="row">
                      <div class="col-md-12 text-right">
                          <div style="padding-top: 20px; font-weight: bold">Total: {{this.convertToRupiah(total + (total * 0.1))}}</div>
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
                    <button v-on:click="cartCancel()" id="btn-checkoutBlue" type="button">Cancel</button>
                  </div>
                  <!-- <div class="col-md-6 total">Total :</div><div class="col-md-12 total">*Belum termasuk PPN</div>
                  <div v-for="(item, index) in dataCart" :key="index">
                    <div class="col-md-6 total" style="padding-left: 80px; display: none">{{item.price}}</div>
                  </div> -->
                  <!-- <div id="resize" class="row">
                    <img src="../assets/img/empty_cart.png" alt="Empty Cart">
                    <div style="padding: 0px 0px 0px 120px; font-weight: bold; font-size: 22px;">Your cart is empty</div>
                    <div style="padding: 0px 0px 0px 120px; color: rgba(190, 195, 202, 0.911); font-size: 14px;">Please add some items from the menu</div>
                  </div> -->
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
// import componentCheckout from '../components/ModalCheckout'
export default {
  name: 'Product',
  data () {
    return {
      listProducts: [],
      dataCart: [],
      btnSearchShow: false, // set default (not showing)
      btnStripShow: false,
      btnForkShow: false,
      listCheckout: [],
      total: 0,
      search: '',
      sort: '',
      tax: ''
    }
  },
  mounted () {
    Axios.get('http://localhost:3000/products?name_product=&param=&sort=&page=&limit=100').then((response) => {
      this.listProducts = response.data
    }).catch((err) => {
      console.log(err)
    })
  },
  // computed: {
  //   filteredSearch () {
  //     return this.listProducts.filter((el) => {
  //       return el.name.toLowerCase().match(this.search.toLowerCase())
  //     })
  //   }
  // },
  components: {
    componentProducts,
    componentAddProduct
    // componentCheckout
  },
  methods: {
    findData: function () {
      const search = this.search ? this.search : 'a'
      Axios.get('http://localhost:3000/products?name_product=' + search + '&param=&sort=&page=&limit=100').then((response) => {
        this.listProducts = response.data
      }).catch((err) => {
        console.log(err)
      })
      // const sort = this.sort?this.sort:'ASC'
      // Axios.get('http://localhost:3000/products?name_product=' + search + '&param=&' + sort + '=&page=&limit=100')
    },
    sortData: function () {
      const sort = this.sort ? this.sort : 'd'
      Axios.get('http://localhost:3000/productsCategory?name_category=' + sort + '&param=&sort=ASC&page=1&limit=100').then((response) => {
        this.listProducts = response.data
      }).catch((err) => {
        console.log(err)
      })
    },
    setTotal: function () {
      this.total = 0
      this.dataCart.forEach((el) => {
        el.price = el.price.replace(/[\W]/g, '')
        el.price = el.price.replace('Rp', '')
        this.total = this.total + (el.price * el.qty)
        el.price = this.convertToRupiah(el.price)
        // console.log(this.dataCart)
      })
    },
    // setTax: function () {
    //   this.tax = 0
    //   this.dataCart.forEach((el) => {
    //     this.total = this.total
    //   })
    // },
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
          image: element.image
        }
        this.dataCart = [...this.dataCart, newData]
      }
      this.setTotal()
    },
    checkout: function () {
      // for (let i = 0; i < element.length; i++) {
      //   const result = {
      //     orders: element[i].id_product
      //   }
      //   console.log(element[i].id_product)
      //   this.listCheckout = [...this.listCheckout, result]
      // }
      // this.dataCart.forEach((item) => {
      //   this.listCheckout = {
      //     invoice: '10933',
      //     cashier: 'Cashier 2',
      //     id_product: item.id_product,
      //     qty: item.qty
      //   }
      // })
      // this.listCheckout = [...this.dataCart, this.listCheckout]
      this.dataCart.forEach((item) => {
        this.listCheckout = {
          // cashier: item.cashier,
          // id_product: item.id_product,
          // image: item.image,
          // invoice: item.invoice,
          // name: item.name,
          // price: item.price,
          // qty: item.qty
        }
      })
      this.listCheckout = [...this.dataCart, this.listCheckout]
      // console.log(this.listCheckout)
      console.log(this.dataCart)
      Axios.post('http://localhost:3000/history', this.listCheckout).then((response) => {
        console.log(response)
      }).catch((err) => {
        console.log(err)
      })
    },

    // this.dataCart.forEach((i) => {
    //   i.id_product = response.data
    // })
    // for (let i = 0; i < response.length; i++) {
    //   const result = {
    //     orders: response.data[i].id_product
    //   }
    // this.dataCart = [...this.dataCart, result]
    // }
    // console.log(response.data)
    // this.listCheckout = response.data
    // const checkoutProducts = this.listCheckout.foreach((i) => {
    //   orders: response[i].id_product
    // })
    // const newData = {
    //   orders: response.data.id_product
    // }
    // this.listCheckout = [...this.listCheckout, checkoutProducts]
    // console.log(this.listCheckout)

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
      console.log(newCart)
      this.dataCart = newCart
      this.setTotal()
    },
    cartCancel: function () {
      // console.log(idProduct)
      // const newCancel = this.dataCart.map((item) => {
      //   return item.id_product !== idProduct
      // })
      // this.dataCart = newCancel
      this.$router.go('/')
    },
    hideModal: function () {
      this.$refs['my-modal'].hide()
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
  height: 150px;
  width: 190px;
}
#resize img{
  padding: 40px 0px 0px 140px;
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
  position: absolute;
  right: 0;
  left: 8px;
  bottom: 6%;
  border-radius: 0px;
  background-color: rgba(242,79,138, 0.8);
  width: 100%;
  color: rgba(255,255,255, 0.8);
}
#btn-checkoutBlue{
  position: absolute;
  right: 0;
  left: 8px;
  bottom: 2%;
  border-radius: 0px;
  background-color: rgba(87,202,213, 0.8);
  width: 100%;
  color: rgba(255,255,255, 0.8);
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
footer{
  position: absolute;
  bottom: 10%;
}
/* @media (min-width: 300px) and (max-width: 767px) {
  .product.col-7{
  }
} */
</style>
