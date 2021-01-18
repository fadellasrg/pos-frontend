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
                <div class="text-center">Menu Details</div>
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
            <div class="col-md-9 text-center">
              <!-- <div v-if="isLoading">
                Loading
              </div> -->
              <!-- <div v-else> -->
                <div class="name">{{result.name}}</div>
                <img :src="result.image" class="figure-img img-fluid rounded" :alt="result.category" />
                <div >Price: {{result.price}}</div>
                <div>Category: {{result.category}}</div>
              <!-- </div> -->
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
      isLoading: true,
      result: {},
      btnStripShow: false
    }
  },
  mounted () {
    this.detailData()
  },
  components: {
    componentAddProduct
  },
  methods: {
    detailData: function () {
      Axios.get('http://localhost:3000/product/' + this.id).then((response) => {
        console.log(response.data)
        this.result = response.data[0]
      }).catch((err) => {
        console.log(err)
      })
      // .finally(() => {
      //   this.isLoading = false
      // })
    },
    btnStrip: function () {
      this.btnStripShow = !this.btnStripShow
    }
  }
}
</script>
<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@500&display=swap');
#side-bar img{
  margin-top: 2px;
  margin-left: 30px;
  margin-bottom: 35px;
  height: 32px;
  width: auto;
}
img{
  height: 190px;
  width: 220px;
}
div{
  font-family: 'Noto Sans JP', sans-serif;
  font-size: 20px;
}
.img-thumbnail{
  margin-left: 10px;
  margin-top: 10px;
  width: 50px;
  height: 50px;
}
.col-md-9 div{
  font-size: 24px;
  font-weight: bold;
}
.col-md-7 div{
  font-size: 30px;
  font-weight: bold;
}
.name{
  font-weight: bold;
  padding-bottom: 15px;
}
.img-strip{
  margin-top: 18px;
}
</style>
