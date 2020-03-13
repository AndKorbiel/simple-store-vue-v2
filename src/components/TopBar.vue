<template>
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
                                <i class="fa fa-trash" v-on:click="$emit('handleItem', item.product, 'remove')"></i>
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
</template>

<script>
    export default {
        name: 'TopBar',
        data: function() {
            return {
                miniCartIsVisible: false
            }
        },
        props: {
            itemsInCart: Array,
            totalCost: Number,
            show: Boolean,
            shopIsOpen: Boolean,
        }
    }
</script>

<style scoped>
    .nav {
        background: rgba(0, 0, 0, 0.76);
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
        padding: 1px 7px;
        color: #303c8a;
        margin-right: 10px;
    }

    .nav .small-cart .circle {
        padding: 1px 2px 1px 7px;
        font-size: 15px;
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

    .closed {
        background: rgba(217, 83, 79, 0.62);
    }
</style>