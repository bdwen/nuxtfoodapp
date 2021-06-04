<template>
    <main class="container">
        <section class="image" :style="`background: url(/${currentItem.img}) no-repeat center center`"/>

        <section class="details">
            <h1>{{ currentItem.item }}</h1>
            <h3>Price: {{ formatPrice(currentItem.price) }}</h3>
            <div class="quantity">
                <input type="number" min="1" v-model="quantity"/>
                <button class="primary" @click="addToCart">Add to Cart {{ cartTotal }}</button>
            </div>
            <fieldset v-if="currentItem.options">
                <legend>
                    <h3>Options</h3>
                </legend>
                <div v-for="option in currentItem.options" :key="option">
                    <input v-model="selectedOption" type="radio" name="option" :id="option" :value="option"/>
                    <label :for="option">{{ option }}</label>
                </div>
            </fieldset>
            <fieldset v-if="currentItem.addOns">
                <legend>
                    <h3>Add Ons</h3>
                </legend>
                <div v-for="addon in currentItem.addOns" :key="addon">
                    <input v-model="selectedAddons" type="checkbox" name="addon" :id="addon" :value="addon"/>
                    <label :for="addon">{{ addon }}</label>
                </div>
            </fieldset>

            <AppToast v-if="cartSubmitted">
                Order submitted <br>
                Check out more <nuxt-link to="/restaurants">restaurants!</nuxt-link>!
            </AppToast>
        </section>

        <section class="options">
            <h3>Description</h3>
            <p>{{ currentItem.description }}</p>

        </section>
    </main>
</template>

<script>
import { mapState } from 'vuex';
import AppToast from '@/components/AppToast.vue';

    export default {
        data() {
            return {
                id: this.$route.params.id,
                quantity: 1,
                selectedOption: '',
                selectedAddons: [],
                cartSubmitted: false,
            }
        },
        components: {
            AppToast
        },
        computed: {
            ...mapState([
                'fooddata',
            ]),
            currentItem() {
                let result;

                for (let i = 0; i < this.fooddata.length; i++) {
                    for (let j = 0; j < this.fooddata[i].menu.length; j++) {
                        if (this.fooddata[i].menu[j].id === this.id) {
                            result = this.fooddata[i].menu[j];
                            break;
                        }
                    }
                }

                return result;
            },
            cartTotal() {
                return this.quantity * this.currentItem.price;
            },
        },
        methods: {
            formatPrice(price) {
                return '$' + price.toFixed(2);
            },
            addToCart() {
                let formOutput = {
                    item: this.currentItem.item,
                    count: this.quantity,
                    options: this.selectedOption,
                    addOns: this.selectedAddons,
                    combinedPrice: this.cartTotal
                }
                this.cartSubmitted = true;
                this.$store.commit('addToCart', formOutput);
            }
        }
    }
</script>

<style lang="scss" scoped>
.container {
    display: grid;
    grid-template-columns: 400px 1fr;
    grid-template-rows: 400px 1fr;
    grid-column-gap: 60px;
    grid-row-gap: 60px;

    width: 1000px;
    margin: 100px auto;
    line-height: 2;
}

.image { 
    grid-area: 1 / 1 / 2 / 2;
}

.details { 
    grid-area: 1 / 2 / 2 / 3;
    position: relative;
}

.options { grid-area: 2 / 1 / 3 / 2; }

.quantity {
    display: flex;
    margin: 20px 0 40px;
}
</style>