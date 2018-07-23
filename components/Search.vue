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
            attendanceOptions: {
                'очная': '/ochno/',
                'очно-заочная': '/ochno-zaochno/',
                'заочная': '/zaochno/',
                'дистанционная': '/distancionno/',
            },
            submitUrlParts:{
                uzOptions: '',
                specOptions: '',
                searchKey: '',
            },
            searchMessage:''
        }),
        methods: {
            click: function(){
                // console.log(this.$route);
            },
            addUzOptionToUrl: function(option)
            {
                this.submitUrlParts.uzOptions = this.uzOptions[option];
            },
            addSearchMessageToUrl: function() {
                /*массив из ключей specOptions*/
                let searchParams = this.allOptionsKeys;
                console.log('searchParams', searchParams);

                let chosenSearchOptionUrl, //часть урла, которая будет сформирована после обработки сообщения из поисковой строки
                    optionSection; //раздел справочника, к которому относится введенное сообщение из поисковой строки

                if(this.searchMessage.length>0) {
                    if(Object.keys(searchParams).includes(this.searchMessage)){
                        /*если введенный в поиск запрос найден в справочнике*/
                        optionSection = searchParams[this.searchMessage];
                        let section = this[optionSection];
                        chosenSearchOptionUrl = section[this.searchMessage];
                    }
                }

                /*Если введенный в поиск запрос найден в справочнике */

                if(chosenSearchOptionUrl) {
                    this.submitUrlParts[optionSection] = chosenSearchOptionUrl;
                }
                /*если введенный в поиск запрос НЕ найден в справочнике */
                else {
                    this.submitUrlParts[optionSection] = '/' + this.searchMessage;
                }


                if(!this.submitUrlParts.uzOptions) {
                    this.submitUrlParts.searchKey = 'search';
                }

            }
        },
        computed: {
            allOptionsKeys: function(){
                let optionsKeys = {};
                for(let key in this.specOptions) {
                    optionsKeys[key] = 'specOptions';
                }
                for(let key in this.attendanceOptions) {
                    optionsKeys[key] = 'attendanceOptions';
                }
                return optionsKeys;
            },
            submitUrl: function() {
                return this.submitUrlParts.uzOptions + this.submitUrlParts.searchKey + this.submitUrlParts.specOptions;
            }
        },
    }
</script>