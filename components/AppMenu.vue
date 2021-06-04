<template>
    <nav>
        <ul>
            <li><nuxt-link exact to="/"><AppLogo /></nuxt-link></li>
            <li><nuxt-link exact to="/restaurants">Restaurants</nuxt-link></li>
        </ul>
        <nuxt-link exact to="/cart">Cart</nuxt-link>
        <button v-if="totalItems > 0" class="smallnum">{{ totalItems }}</button>
    </nav>
</template>

<script>
import AppLogo from '@/components/AppLogo.vue';
import { mapGetters } from 'vuex';

export default {
        components: {
            AppLogo,
        },
        computed: {
            ...mapGetters(['totalItems'])
        },
    }
</script>

<style lang="scss" scoped>

</style>