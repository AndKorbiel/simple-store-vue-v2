<template>
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
                    <input type="text" id="formMax" class="form-control"
                           v-model="maximum"
                           v-on:change="changeMaximum"
                    />
                </div>
                <div class="form-inline">
                    <label class="font-weight-bold" for="formMax">Open / Close</label>
                    <button class="btn btn-danger"
                            v-on:click="$emit('closeOpenShop')">
                        {{ shopIsOpen ? 'Close the shop' : 'Open the shop' }}
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'OptionsPanel',
        props: {
            shopIsOpen: Boolean,
            maximum: Number,
            message: String
        },
        methods: {
            changeMaximum: function(ev) {
                this.$emit('changeMaximum', ev.target.value)
            }
        }
    }
</script>

<style scoped>
    .message {
        text-align: center;
        margin-bottom: 20px;
    }

    #app .options-panel {
        background: rgba(0, 0, 0, 0.54);
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
</style>