<template>
  <div v-if="data !== null">
    <div class="product-item-holder" v-if="data.product !== null">
      <div class="product-image" v-if="data.product.featured !== null">
        <img :src="config.BACKEND_URL + data.product.featured[0].url" class="main-image" >
      </div>
      <div class="product-image" v-if="data.product.featured === null">
        <i class="fa fa-image" v-if="data.product.featured === null"></i>
      </div>
      <div class="product-details">
        <div class="product-title">
          <h5 style="line-height: 30px;">
            {{data.product.title}}
            <button class="btn btn-danger pull-right" @click="remove(data.id)"><i class="fa fa-trash"></i></button>
          </h5>
        </div>
        <div v-if="data.price !== null" class="product-row text-primary">
          <label>PHP {{data.price}} x {{data.qty}} = PHP {{parseFloat(data.price) * parseFloat(data.qty)}}</label>
        </div>
        <div class="product-row">
          <label class="qty-action" @click="minus(data)" v-if="data.qty > 1"><i class="fas fa-minus"></i></label>
          <label class="qty-label">{{data.qty}}</label>
          <label class="qty-action" @click="plus(data)"><i class="fas fa-plus"></i></label>
        </div>
      </div>
    </div>
  </div>
</template>
<style scoped>
  .product-item-holder{
    width: 100%;
    float: left;
    height: 150px;
  }
  .product-image{
    float: left;
    height: 150px;
    text-align: center;
    width: 30%;
  }
  .product-image .main-image{
    height: 150px;
    float: left;
    width: 150px;
  }
  .product-image .fa-image{
    font-size: 100px;
    line-height: 150px;
    width: 150px;
  }
  .product-details{
    min-height: 50px;
    width: 70%;
    float: left;
    overflow-y: hidden;
  }
  .product-title{
    width: 100%;
    float: left;
    min-height: 10px;
    overflow-y: hidden;
    padding-top: 5px;
  }
  .product-row{
    width: 100%;
    float: left;
    height: 40px;
    overflow-y: hidden;
    font-weight: 600;
    line-height: 40px;
  }
  .qty-action{
    font-size: 20px;
    line-height: 38px;
    border-radius: 5px;
    padding-left: 10px;
    padding-right: 10px;
    border: solid 1px #ddd;
    color: #ffaa81;
  }
  .qty-action:hover{
    border: solid 1px #ffaa81;
    cursor: pointer;
  }
  .qty-label{
    font-size: 20px;
    line-height: 38px;
    border-radius: 5px;
    padding-left: 10px;
    padding-right: 10px;
    border: solid 1px #ddd;
  }
</style>
<script>
import ROUTER from 'src/router'
import AUTH from 'src/services/auth'
import CONFIG from 'src/config.js'
import axios from 'axios'
export default {
  mounted(){
  },
  data(){
    return {
      user: AUTH.user,
      config: CONFIG,
      errorMessage: null
    }
  },
  props: ['data', 'route'],
  methods: {
    redirect(parameter){
      ROUTER.push(parameter)
    },
    plus(data){
      let qty = parseInt(data.qty) + 1
      let parameter = {
        id: data.id,
        qty: qty
      }
      $('#loading').css({display: 'block'})
      this.APIRequest(this.route + '/update', parameter).then(response => {
        $('#loading').css({display: 'none'})
        AUTH.checkAuthentication(null)
        this.$parent.retrieve()
      })
    },
    minus(data){
      let qty = parseInt(data.qty) - 1
      let parameter = {
        id: data.id,
        qty: qty
      }
      $('#loading').css({display: 'block'})
      this.APIRequest(this.route + '/update', parameter).then(response => {
        $('#loading').css({display: 'none'})
        AUTH.checkAuthentication(null)
        this.$parent.retrieve()
      })
    },
    remove(id){
      let parameter = {
        id: id
      }
      $('#loading').css({display: 'block'})
      this.APIRequest(this.route + '/delete', parameter).then(response => {
        $('#loading').css({display: 'none'})
        AUTH.checkAuthentication(null)
        this.$parent.retrieve()
      })
    }
  }
}
</script>
