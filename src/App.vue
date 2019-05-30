<template>
  <div id="app">
    <div class="container-fluid">
      <transition name="fade" v-if="show">
        <div v-bind:class="shopIsOpen === true ? 'open' + ' row nav': 'closed' + ' row nav' ">
          <div class="col-sm-12">
            <button class="cartButton btn btn-info" v-on:click="miniCartIsVisible = !miniCartIsVisible">
              <span class="circle">{{ itemsInCart.length }}</span>
              <i class="fa fa-shopping-cart"></i> ${{totalCost}}
              <i class="fa fa-sort-down"></i>
            </button>
            <div class="small-cart" v-if="miniCartIsVisible">
              <ul v-for="(item, index) in itemsInCart" v-bind:key="index">
                <li>
                  <i class="fa fa-trash" v-on:click="handleItem(item.product, 'remove')"></i>
                  <span class="circle">{{ item.qty }} </span>
                  {{ item.product.name }}
                  <span class="price"> ${{ item.product.price * item.qty }}</span>
                </li>
              </ul>
              <p>Total: <span class="totalPrice">${{totalCost}}</span></p>
            </div>
          </div>
        </div>
      </transition>
    </div>
    <div class="container">
      <div class="row">
        <div class="col-md-3 options-panel">
          <div class="row message">
            <div class="col-md-12">
              <h4>Current status:</h4>
            </div>
            <div class="col-md-12" v-if="shopIsOpen === true">
              <h2 v-on:click="show = !show">{{ message }}</h2>
            </div>
            <div class="col-md-12" v-else="">
              <h2>Shop is closed!</h2>
            </div>
          </div>
          <div class="row options">
            <div class="col-md-12">
              <h4>Options</h4>
              <div class="form-inline">
                <label class="font-weight-bold" for="formMax">Max price</label>
                <input type="text" id="formMax" class="form-control" v-model="maximum" />
              </div>
              <div class="form-inline">
                <label class="font-weight-bold" for="formMax">Open / Close</label>
                <button class="btn btn-danger"
                        v-on:click="closeOpenShop">
                  {{ shopIsOpen ? 'Close the shop' : 'Open the shop' }}
                </button>
              </div>
            </div>
          </div>
        </div>
        <div class="col-md-8 offset-md-1">
          <div v-for="item in products" v-if="item.price <= Number(maximum) || Number(maximum) == '' ">
            <div class="row product">
              <div class="col-md-3">
                <img class="img-responsive"
                     v-bind:src="item.img"
                />
              </div>
              <div class="col-md-9">
                <h2> {{ item.name }}</h2>
                <p>{{ item.desc }}</p>
                <h3>${{ item.price }}</h3>
                <button
                        v-bind:class="shopIsOpen === true ? 'btn btn-info' : 'btn disabled' "
                        v-on:click="handleItem(item, 'add')">
                  Add to cart
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
  body {
    background-color: #303c29;
    background-image: url("https://i.etsystatic.com/12928818/r/il/bf6df6/1272227789/il_794xN.1272227789_ko7l.jpg");
    background-repeat: repeat;
  }

  #app {
    margin-top: 100px;
  }

  h2, .h2 {
    font-size: 24px;
  }

  #app .container-fluid {
    margin-bottom: 40px;
  }

  .product {
    background: rgba(93, 93, 93, 0.27);
    padding: 15px;
    color: white;
    box-shadow: 0 6px 6px rgba(0, 0, 0, 0.41);
    margin-bottom: 15px;
    align-items: center;
    justify-content: center;
    display: flex;
  }

  .product h2 {
    color: #5d6757;
  }

  .product h3 {
    color: #d43f3a;
    float: left;
  }

  .product .btn {
    margin-left: 16px;
  }

  .product img {
    max-width: 100%;
  }

  .message {
    text-align: center;
    margin-bottom: 20px;
  }

  #app .options-panel {
    background: rgba(93, 93, 93, 0.27);
    padding: 30px 10px;
    text-align: center;
    color: white;
    border: 3px solid #21291c;
  }

  .options {
    text-align: left;
  }

  .options h4 {
    text-align: center;
    border-top: 1px solid white;
    padding-top: 14px;
    font-size: 14px;
  }

  .form-inline {
    display: flex;
    margin-bottom: 1em;
    align-items: center;
    justify-content: center;
  }

  .options label {
    width: 40%;
    font-size: 14px;
  }

  .options .form-inline input,
  .options .form-inline button {
    width: 60%;
  }

  .nav {
    background: rgba(93, 93, 93, 0.27);
    padding: 10px;
    text-align: right;
    position: fixed;
    top: 0;
    z-index: 9;
    width: 100%;
    border-bottom: 3px solid #21291c;
  }
  .nav .btn {
    font-size: 16px;
    font-weight: bold;
  }

  .nav .fa {
    margin-right: 10px;
  }

  .nav .fa-sort-down {
    margin-left: 5px;
    font-size: 22px;
    line-height: 12px;
    vertical-align: top;
  }

  .nav .circle {
    background: white;
    border-radius: 30px;
    padding: 2px 7px;
    color: #303c8a;
    margin-right: 10px;
  }

  .nav .small-cart .circle {
    padding: 2px 2px 2px 7px;
    font-size: 15px;
  }

  .closed {
    background: rgba(217, 83, 79, 0.62);
  }

  .fade-enter-active, .fade-leave-active {
    transition: opacity .5s;
  }
  .fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
    opacity: 0;
  }

  .small-cart {
    width: 200px;
    position: absolute;
    height: auto;
    background: rgba(255, 255, 255, 0.29);
    padding: 15px;
    border: 2px solid #21291c;
    box-shadow: 0 2px 5px #61616199;
    right: 15px;
    top: 43px;
    color: white;
    font-size: 17px;
  }

  .small-cart ul {
    list-style: none;
    padding: 0;
  }

  .small-cart ul li {
    border-bottom: 1px solid #cecece;
    padding-bottom: 10px;
  }

  .small-cart ul li .price,
  .small-cart .totalPrice {
    font-weight: bold;
  }

  .btn-info {
    background-color: #303c29;
    border-color: #303c29;
  }

  .btn-info:hover  {
    background-color: #5c734e;
    border-color: #5c734e;
  }

  .btn.disabled, .btn[disabled], fieldset[disabled] .btn {
    color: #4e4e4e;
  }

</style>

<script>

    export default {
        name: 'app',
        data: function() {
            return (
                {
                    products : [
                        {
                            name: 'Hat',
                            id: 1,
                            desc: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nunc accumsan tincidunt nunc, sed vulputate lectus pulvinar ut. Suspendisse nec eros vehicula, gravida orci ut, ullamcorper orci. Vestibulum et egestas turpis. Proin mauris nisi, posuere id libero vulputate, tempus iaculis mi. Sed et mollis lorem.',
                            img: 'https://cdn.shopify.com/s/files/1/1352/9125/products/oh_heck_-_gray_1200x1200.png?v=1542925362',
                            price: 99
                        },
                        {
                            name: 'Bag',
                            id: 2,
                            desc: 'Aenean nec vestibulum libero, vel suscipit mi. Vestibulum ultrices, sapien vel cursus venenatis, magna ipsum gravida ipsum, a placerat dolor tellus sollicitudin est. Nunc odio diam, tempor ut tortor ut, facilisis lobortis magna. Aliquam non ipsum est. Nam magna nibh, convallis convallis risus non, posuere malesuada orci. ',
                            img: 'https://img.pngio.com/backpack-png-image-backpack-png-2000_2000.png',
                            price: 50
                        },
                        {
                            name: 'Machine Gun',
                            id: 3,
                            desc: 'Donec quam orci, ultrices et elementum eget, cursus non purus. Vivamus vel laoreet tortor. Vestibulum quis sapien vel velit imperdiet posuere. Vivamus suscipit eget lectus id pulvinar. ',
                            img: 'https://i.ya-webdesign.com/images/fortnite-hand-cannon-png-5.png',
                            price: 199
                        },
                    ],
                    shopIsOpen: true,
                    message: 'Shop is open, buy!!!',
                    maximum: 199,
                    itemsInCart: [],
                    totalCost: 0,
                    show: true,
                    miniCartIsVisible: false
                }
            )

        },

        methods: {
            handleItem: function(product, condition) {
                var currentProduct;
                var existing = this.itemsInCart.filter(function (item, index) {
                    if (item.product.id == Number(product.id)) {
                        currentProduct = index;
                        return true
                    }
                    else {
                        return false
                    }
                });

                if (existing.length && condition === 'add') {
                    this.itemsInCart[currentProduct].qty++;
                    this.totalCost = this.totalCost + product.price;
                }
                else if (existing.length && condition === 'remove') {
                    this.itemsInCart[currentProduct].qty--;
                    this.totalCost = this.totalCost - product.price;

                    if (this.itemsInCart[currentProduct].qty === 0) {
                        this.itemsInCart.splice(currentProduct, 1)
                    }
                }
                else {
                    this.itemsInCart.push({product: product, qty: 1});
                    this.totalCost = this.totalCost + product.price;
                }

            },
            closeOpenShop: function() {
                this.shopIsOpen ? this.shopIsOpen = false : this.shopIsOpen = true;
            }
        }
    }



</script>


<!--<template>-->
  <!--<div id="app">-->
    <!--<img alt="Vue logo" src="./assets/logo.png">-->
    <!--<HelloWorld msg="Welcome to Your Vue.js App"/>-->
  <!--</div>-->
<!--</template>-->

<!--<script>-->
<!--import HelloWorld from './components/HelloWorld.vue'-->

<!--export default {-->
  <!--name: 'app',-->
  <!--components: {-->
    <!--HelloWorld-->
  <!--}-->
<!--}-->
<!--</script>-->

<!--<style>-->
<!--#app {-->
  <!--font-family: 'Avenir', Helvetica, Arial, sans-serif;-->
  <!-- -webkit-font-smoothing: antialiased;-->
  <!-- -moz-osx-font-smoothing: grayscale;-->
  <!--text-align: center;-->
  <!--color: #2c3e50;-->
  <!--margin-top: 60px;-->
<!--}-->
<!--</style>-->
