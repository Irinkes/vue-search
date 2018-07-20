<template>
    <div class="search" @click="click">
        <search-uz-options v-bind:uzOptions ='uzOptions' @receiveOption="addUzOptionToUrl"/>
        <input type="text" v-model="searchMessage" @change='addSearchMessageToUrl' placeholder="Search goes here"/>
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
            specOptions: {
                'искусство': '/art/',
                'биология': '/biology/',
                'юриспруденция': '/jurisprudence/',
                'филология': '/filologiya/',
            },
            submitUrlParts:{
                uzOption: '',
                specOption: '',
            },
            searchMessage:''
        }),
        methods: {
            click: function(){
                console.log(this.$route);
            },
            addUzOptionToUrl: function(option)
            {
                this.submitUrlParts.uzOption = this.uzOptions[option];
            },
            addSearchMessageToUrl: function() {
                /*массив из ключей specOptions*/
                let specLabels = Object.keys(this.specOptions);
                let chosenSearchOptionUrl;

                if(this.searchMessage.length>0) {
                    if(specLabels.includes(this.searchMessage)){
                        chosenSearchOptionUrl =this.specOptions[this.searchMessage];
                    }
                }



                    if(chosenSearchOptionUrl) {
                        this.submitUrlParts.specOption = chosenSearchOptionUrl;
                    }
                    this.submitUrlParts.specOption = '/search/' + this.searchMessage;

            }
        },
        computed: {
            submitUrl: function() {
                return this.submitUrlParts.uzOption + this.submitUrlParts.specOption;
            }
        },
    }
</script>