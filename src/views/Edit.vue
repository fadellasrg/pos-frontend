<template>
    <div>
        <div class="container-fluid">
        <div class="row">
            <div class="col-md-2 col-sm-4">
                <nav class="navbar navbar-light">
                    <img @click="btnStrip()" src="../assets/img/strip.png" class="img-thumbnail img-strip" alt="Menu">
                </nav>
            </div>
            <div class="col-md-7 col-sm-6 text-center">
                <div class="text-center">Edit</div>
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
                        <b-modal hide-footer id="modal-center" centered title="Add Item">
                          <componentAddProduct/>
                        </b-modal>
                      </div>
                  </nav>
              </div>
            </div>
            <div id="editData" class="col-md-10 text-center">
              <div style="height: 240px" class="container-fluid">
                <!-- <div v-for="(element, index) in listEdit" v-bind:key="index"> -->
                <input type="text" v-model="listEdit.name" class="form-control" placeholder="Name">
                <input type="text" v-model="listEdit.image" class="form-control" placeholder="Image">
                <input type="text" v-model="listEdit.price" class="form-control" placeholder="Price">
                <select v-model="listEdit.id_category" style="margin-top: 0px; width: 98%" class="form-select form-select-lg mb-3">
                  <option disabled value="">Category</option>
                  <option value="1">Dessert</option>
                  <option value="2">Drink</option>
                  <option value="3">Food</option>
                </select>
                <!-- </div> -->
              </div>
              <router-link to="/">
                <button class="btn add-btn" v-on:click="update()">Update</button>
              </router-link>
              <router-link to="/">
                <button class="btn cancel-btn">Cancel</button>
              </router-link>
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
      id: this.$route.params.id,
      btnStripShow: false,
      listEdit: {
        name: '',
        image: '',
        price: '',
        id_category: ''
      }
    }
  },
  mounted () {
    this.getProduct()
  },
  methods: {
    btnStrip: function () {
      this.btnStripShow = !this.btnStripShow
    },
    hideModal: function () {
      this.$refs['my-modal'].hide()
    },
    update: function () {
      const finalData = {
        name: this.listEdit.name,
        image: this.listEdit.image,
        price: this.listEdit.price,
        id_category: this.listEdit.id_category
      }
      console.log(this.listEdit)
      Axios.patch('http://localhost:3000/products/' + this.id, finalData).then((response) => {
        console.log(response)
      }).catch((err) => {
        console.log(err)
      })
    },
    getProduct: function () {
      Axios.get('http://localhost:3000/product/' + this.id).then((response) => {
        // this.listProducts = response.data
        this.listEdit = response.data.data[0]
      }).catch((err) => {
        console.log(err)
      })
    }
  },
  // watch: {
  //   update: function () {
  //   Axios.patch('http://localhost:3000/products/' + this.id, this.listEdit).then((response) => {
  //     console.log(response)
  //   }).catch((err) => {
  //     console.log(err)
  //   })
  // }
  // },
  components: {
    componentAddProduct
  }
  // mounted () {
  // }
}
</script>
<style scoped>
#side-bar img{
  margin-top: 2px;
  margin-left: 35px;
  margin-bottom: 35px;
  height: 32px;
  width: auto;
}
#editData input{
  margin: 15px;
}
.col-md-7 div{
  font-size: 30px;
  font-weight: bold;
  padding-top: 0px;
}
.img-strip{
  margin-top: 18px;
}
.add-btn{
    background-color: rgba(242,79,138, 0.8);
    width: 18%;
    color: rgba(255,255,255, 0.8);
    margin: 0;
    border-radius: 6px;
    margin-right: 100px;
}
.cancel-btn{
    background-color: rgba(87,202,213, 0.8);
    width: 18%;
    color: rgba(255,255,255, 0.8);
    margin: 0;
    border-radius: 6px;
}
nav img{
    margin: 20px 0px 0px 20px;
}
</style>
