<template>
    <div class="search" @click="click">
        <search-uz-options v-bind:uzOptions ='uzOptions' @receiveOption="buildSubmitUrl"/>
        <input type="text" placeholder="Search goes here"/>
        <router-link :to="{path: submitUrl}" class="btn search-submit">Найти</router-link>
    </div>
</template>


<style lang="scss">
    @import "~/css/main.scss";

    .search input{
        width: 300px;
        height: 42px;
        line-height: 42px;
        padding: 10px;
        border: 1px solid #ccc;
    }
    .search-submit {
        height: 44px;
        line-height: 42px;
        padding: 0 15px;
        margin-left: 15px;
        cursor: pointer;
        display: inline-block !important;
    }
</style>
<script>
    import SearchUzOptions from "./SearchUzOptions";
    export default {
        components: {SearchUzOptions},
        name: 'search',
        data: ()=> ({
            uzOptions: {
                'вузы': '/for-abiturients/vuz',
                'колледжи': '/for-abiturients/college',
                'подготовительные курсы': '/for-abiturients/prepare',
                'второе высшее': '/for-specialists/',
                'магистратура': '/for-specialists/',
                'аспирантура': '/for-specialists/'
            },
            submitUrl: '/'
        }),
        methods: {
            click: function(){
                console.log(this.$route);
            },
            buildSubmitUrl: function(option)
            {
                this.submitUrl = this.uzOptions[option];
            }
        },
        computed: {

        }
    }
</script>