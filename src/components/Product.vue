<template>
    <div>
        <div id="product" class="col-12 text-justify">
            <img v-on:click="addToCart(element), showIcon()" :src="element.image" class="figure-img img-fluid rounded" :alt="element.category" />
            <div>{{element.name}}</div>
            <div class="price">{{this.convertToRupiah(element.price)}}</div>
            <router-link :to="'/detail/' + element.id_product">
              <button id="detail-btn" type="button" class="btn">Detail</button>
            </router-link>
            <router-link :to="'/edit/' + element.id_product">
              <button id="edit-btn" class="btn">Edit</button>
            </router-link>
            <div id="content" v-if="showIcons">
              <svg xmlns="http://www.w3.org/2000/svg" width="40px" height="40px" fill="white" class="bi bi-check-circle" viewBox="0 0 16 16">
                <path d="M8 15A7 7 0 1 1 8 1a7 7 0 0 1 0 14zm0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16z"/>
                <path d="M10.97 4.97a.235.235 0 0 0-.02.022L7.477 9.417 5.384 7.323a.75.75 0 0 0-1.06 1.06L6.97 11.03a.75.75 0 0 0 1.079-.02l3.992-4.99a.75.75 0 0 0-1.071-1.05z"/>
              </svg>
            </div>
        </div>
    </div>
</template>
<script>
// import func from '../../vue-temp/vue-editor-bridge'
export default {
  props: ['element'],
  data () {
    return {
      showIcons: false
    }
  },
  methods: {
    addToCart: function (element) {
      this.$emit('actionCart', element)
    },
    showIcon: function () {
      this.showIcons = true
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
    }
  }
}
</script>
<style>
#detail-btn{
  background-color: rgba(240, 51, 120, 0.8);
  /* width: 40%; */
  color: rgba(255,255,255, 0.8);
  margin: 0;
  margin-top: 5px;
}
#edit-btn{
  background-color: rgba(87,202,213, 0.8);
  /* width: 40%; */
  color: rgba(255,255,255, 0.8);
  margin-top: 5px;
  margin-left: 5px;
}
svg{
    position: absolute;
    left: 50%;
    top: 30%;
    transform: translate(-50%, -50%);
}
#content::after{
    content: "";
    width: 165px;
    height: 130px;
    background-color: #333;
    position: absolute;
    top: 20px;
    left: 20px;
    right: 0;
    bottom: 0;
    opacity: 0.5;
    border-radius: 5px;
}
</style>
