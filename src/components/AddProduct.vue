<template>
    <div>
      <div>
        <b-modal ref="my-modalAdd" hide-footer id="modal-center" centered title="Add Item">
          <div style="height: 240px" class="container-fluid">
            <input type="text" v-model="formProducts.name" class="form-control" placeholder="Name">
            <input type="text" v-model="formProducts.image" class="form-control" placeholder="Image">
            <input type="text" v-model="formProducts.price" class="form-control" placeholder="Price">
            <input type="text" v-model="formProducts.id_category" class="form-control" placeholder="ID_category">
        </div>
        <button class="btn add-btn" v-on:click="addProduct()">Add</button>
        <button class="btn cancel-btn" v-on:click="hideModalAdd()">Cancel</button>
        </b-modal>
      </div>
    </div>
</template>

<script>
import Axios from 'axios'
export default {
  name: 'AddProduct',
  data () {
    return {
      formProducts: {
        name: '',
        image: '',
        price: '',
        id_category: ''
      }
      // btnClose: false
    }
  },
  methods: {
    addProduct: function () {
      Axios.post('http://localhost:3000/products', this.formProducts).then((response) => {
        console.log(response)
        // this.$router.push({ path: '/' })
        this.$router.go('/')
      }).catch((err) => {
        console.log(err)
      })
    },
    hideModalAdd: function () {
      this.$refs['my-modalAdd'].hide()
    }
  }
}
</script>
<style scoped>
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
input{
  margin: 10px 0px 0px 0px;
}
button{
  margin-top: 20px;
  position: absolute;
  right: 15px;
  bottom: 20px;
}
</style>
