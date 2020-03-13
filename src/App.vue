<template>
  <div id="app">
    <TopBar
            v-bind:itemsInCart=itemsInCart
            v-bind:totalCost=totalCost
            v-bind:show=show
            v-bind:shopIsOpen=shopIsOpen
            v-on:handleItem="handleItem">
    </TopBar>
    <div class="container">
      <div class="row">
        <OptionsPanel
                v-bind:shopIsOpen=shopIsOpen
                v-bind:maximum=maximum
                v-bind:message=message
                v-on:closeOpenShop="closeOpenShop"
                v-on:changeMaximum="changeMaximum"
                v-model="maximum">
        </OptionsPanel>
        <ProductsList
                v-bind:products="products"
                v-bind:maximum="maximum"
                v-bind:shopIsOpen="shopIsOpen=shopIsOpen"
                v-on:handleItem="handleItem">
        </ProductsList>
      </div>
    </div>
  </div>
</template>

<style>
  body {
    background-color: #303c29;
    background-image: url("assets/bg.jpg");
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

  .closed {
    background: rgba(217, 83, 79, 0.62);
  }

  .fade-enter-active, .fade-leave-active {
    transition: opacity .5s;
  }
  .fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
    opacity: 0;
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

    import TopBar from "./components/TopBar";
    import OptionsPanel from './components/OptionsPanel';
    import ProductsList from "./components/ProductList";

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
                    show: true
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
            },
            changeMaximum: function(value) {
                this.maximum = value;
            }
        },
        components: {
            ProductsList,
            TopBar,
            OptionsPanel
        }
    }

</script>

