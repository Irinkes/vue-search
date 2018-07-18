<template>
    <section class="container" @click="click">
        <div>
            <app-logo/>
            <h1 class="title">
                vue-test
            </h1>
            <h2 class="subtitle">
                test user page
            </h2>
            <img src="~assets/cat.jpg">
        </div>
    </section>
</template>

<script>
    import AppLogo from '~/components/AppLogo.vue'

    export default {
        components: {AppLogo},
        data: ()=> ({

        }),
        methods: {
            click: function(){
                console.log(this.$route);
            }
        },
        computed: {

        }
    }
</script>

<style>
    .container {
        min-height: 100vh;
        display: flex;
        justify-content: center;
        align-items: center;
        text-align: center;
    }

    .title {
        font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; /* 1 */
        display: block;
        font-weight: 300;
        font-size: 100px;
        color: #35495e;
        letter-spacing: 1px;
    }

    .subtitle {
        font-weight: 300;
        font-size: 42px;
        color: #526488;
        word-spacing: 5px;
        padding-bottom: 15px;
    }

</style>