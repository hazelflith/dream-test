<template>
  <div id="app">
    <main>
      <div class="title">
        <h1> Products </h1>
      </div>
      <div class="row">
        <div v-for="product in products" :key="product.id" class="container">
          <div v-if=" product.stock < 10" class="number-container">
            <h1 class="number">{{product.stock}}</h1>
          </div>
          <div v-else class="number-container-ok">
            <h1 class="number-ok">{{product.stock}}</h1>
          </div>
          <div class="text-container">
            <div class="sub-container">
            <h3> {{product.name}} </h3>
            </div>
            <div class="sub-container">
              <h5> Total Sold </h5>
              <div v-if="Math.max(product.sold === Math.max(...products.map(product => product.sold), 0))" class="subnumber-container-highest">
                <h6> {{product.sold}} </h6>
              </div>
              <div v-else class="subnumber-container">
                <h6> {{product.sold}} </h6>
              </div>
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>

export default {
  data(){
    return{
      products: [],
      sales: []
    }
  },
   async mounted(){
    let fetchedProducts = []
    await fetch('http://localhost:3000/products/')
    .then(res=> res.json())
    .then(data => {
      fetchedProducts = data
      for (const product of fetchedProducts) {
        product.sold = 0
      }
    })

    await fetch('http://localhost:3000/sales/')
    .then(res=> res.json())
    .then(data => this.sales = data)
    .then(() => {
      for (const sale of this.sales) {
        fetchedProducts.find(product => product.id === sale.product_id).sold += sale.qty
      }
    })
    .catch(err=> console.log(err.message))
    .finally(() => {
      this.products = fetchedProducts
    })

    // for (const product of this.products){
    //   product.sold = 0
    // }
    // for (const sale in this.sales) {
    //   this.products.find(product => product.id === sale.product_id).sold += sale.qty
    // }
  },
  name: 'App'
}
</script>

<style>

  @import url('https://fonts.googleapis.com/css?family=Poppins');

  *{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  body{
    font-family: 'Poppins';
    background-color: #F7F7F7;
  }

  main{
    position: fixed;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    min-height: 100vh;
    padding: 100px;
  }
  .title{
    padding-bottom :30px;
  }
  .row {
  width: 100%;
  display: flex;
  flex-direction: row;
  justify-content: center;
  }
  
  .container{
    width: 198px;
    height: auto;
    margin:30px;
    margin-left:0px;

    background: #FFFFFF;
    box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.05);
    border-radius: 10px;

  }
  .sub-container{
    margin-left: 30px;
    padding-bottom:10px;
    padding-top:10px;
    display: flex;
    flex-direction: row;
  }
  .number-container{
    width: 134px;
    height: 134px;
    margin:30px;
    margin-bottom:20px;
    padding: 30px;

    background: #FFEBEE;
    border-radius: 10px;
  }
  .number-container-ok{
    width: 134px;
    height: 134px;
    margin:30px;
    margin-bottom:20px;
    padding: 30px;

    background: #F3FBFF;
    border-radius: 10px;
  }
  .subnumber-container{
    width: 38px;
    height: 27px;

    background: #F3F3F3;
    border-radius: 10px;
    margin-left:33px;
    margin-bottom:5px;
    margin-top: -5px;
    padding:6px;
  }
  .subnumber-container-highest{
    width: 38px;
    height: 27px;

    background: #CEEFE8;
    color: #3D7969;
    border-radius: 10px;
    margin-left:33px;
    margin-bottom:5px;
    margin-top: -5px;
    padding:6px;
  }
  .number{
    color: #F1413D;
    text-align: center;
    font-size: 48px;
  }
  .number-ok{
    color: #1191ED;
    text-align: center;
    font-size: 48px;
  }
  h1{
    font-weight:800;
  }
  h6{
    text-align : center;
  }
  .text-container{
    width:auto;
    margin-bottom:20px;
    
  }
  @media (max-width: 700px) {
    h3{
      font-size: 18px;
    }
    h5{
      font-size: 12px;
    }
    .text-container{
      width:auto;
      margin-bottom:0px;
      padding-top: 10px;
  }
    main{
    position: fixed;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    min-height: 100vh;
    padding: 0px;
    padding-top: 50px;
    }
    .title{
      padding-bottom :10px;
    }
    .row {
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    }
    
    .container{
      width: auto;
      height: auto;
      margin:10px;
      margin-left:0px;
      margin-right:0px;

      background: #FFFFFF;
      box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.05);
      border-radius: 10px;
      display: flex;
      flex-direction: row;

    }
    .sub-container{
      width: 200px;
      margin-left: 20px;
      margin-right: 10px;
      padding-bottom:5px;
      padding-top:10px;
      display: flex;
      flex-direction: row;
    }
    .number-container{
      width: 75px;
      height: 75px;
      margin:20px;
      margin-right:5px;
      padding: 0px;
      padding-top: 20px;
      padding-bottom: 20px;

      background: #FFEBEE;
      border-radius: 10px;
    }
    .number-container-ok{
      width: 75px;
      height: 75px;
      margin:20px;
      margin-right:5px;
      padding: 0px;
      padding-top: 20px;
      padding-bottom: 20px;

      background: #F3FBFF;
      border-radius: 10px;
  }
    .subnumber-container{
      width: 35px;
      height: 27px;

      background: #F3F3F3;
      border-radius: 10px;
      margin-left:30px;
      margin-bottom:5px;
      margin-top: -5px;
      padding:6px;
    }
    .subnumber-container-highest{
    width: 35px;
    height: 27px;

    background: #CEEFE8;
    color: #3D7969;
    border-radius: 10px;
    margin-left:30px;
    margin-bottom:5px;
    margin-top: -5px;
    padding:6px;
  }
    .number{
      color: #F1413D;
      text-align: center;
      font-size: 24px;
    }
    .number-ok{
    color: #1191ED;
    text-align: center;
    font-size: 24px;
  }
  }

</style>
