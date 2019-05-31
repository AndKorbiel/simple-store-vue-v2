<template>
    <div class="col-md-8 offset-md-1">
        <div v-for="item in products"
             v-if="item.price <= Number(maximum) || Number(maximum) == '' "
             v-bind:key="item.index">
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
                            v-on:click="$emit('handleItem', item, 'add')">
                        Add to cart
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
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
</style>

<script>
    export default {
        name: 'ProductsList',
        props: {
            products: Array,
            shopIsOpen: Boolean,
            maximum: Number
        }
    }
</script>