<template>
    <div class="search" @click="click">
        <search-uz-options v-bind:uzOptions ='uzOptions' @receiveOption="addUzOptionToUrl"/>
        <input type="text" v-model="searchMessage" @change='addSearchMessageToUrl' placeholder="Search goes here"/>
        <router-link :to="{path: submitUrl}" class="btn search-submit">Найти</router-link>

        <div style="padding-top:10px; margin-bottom: 10px;">
            <span v-if="selected">You have selected '{{JSON.stringify(selected,null,2)}}'</span>
        </div>
        <!--<input type="text" @input="onInputChange" placeholder='Do youDo you'/>-->
        <vue-autosuggest
                :suggestions="filteredOptions"
                @focus="focusMe"
                @click="clickHandler"
                :on-selected="onSelected"
                :render-suggestion="renderSuggestion"
                :get-suggestion-value="getSuggestionValue"
                :input-props="{id:'autosuggest__input',
                               onInputChange: this.onInputChange,
                               placeholder:'Do you feel lucky, punk?'}"
        />

        <div class="" if="filteredOption">{{JSON.stringify(filteredOptions,null,2)}}</div>


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
    import { VueAutosuggest } from 'vue-autosuggest';


    const axios = require('axios');



    export default {
        components: {SearchUzOptions, VueAutosuggest},
        name: 'search',
        head: {
            title: 'Страница поиска'
        },
        data: ()=> ({
            selected: "",
            filteredOptions: [],
            suggestions: [
                {
                    data: null,
                }
            ],
            info: null,
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
        mounted() {
            axios
                // .get('http://www.json-generator.com/api/json/get/cfFzpwovGW?indent=2')
                .get('http://www.json-generator.com/api/json/get/cgviNzYZWq?indent=2')
                .then(response => (this.suggestions[0].data = response));
        },

        methods: {
            click: function(){
                // console.log('isClient', this.context);
                // console.log('this.info', this.info);
            },
            onSelected(item) {
                this.selected = item;
            },
            clickHandler(item){
                // console.log('item', item);
            },
            renderSuggestion() {
                console.log('suggestion', this.filteredOptions);

                // const character = suggestion.item;
                return (
                    '<div style={{ color: "red" }}>{suggestion}</div>;'
                    );
            },
            getSuggestionValue() {
                console.log('suggestion111', this.filteredOptions);
                // return suggestion.item.name;
            },
            focusMe(e) {
                // console.log('suggestions', this.suggestions);
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

            },
            onInputChange(text, oldText) {

                let jsonData = this.suggestions[0].data.data[0];
                console.log('jsonData', jsonData);


                if (text === null) {
                    /* Maybe the text is null but you wanna do
                    * something else, but don't filter by null.
                    */
                    return;
                }


                let filteredJsonData = [];

                for(let item in jsonData) {
                    filteredJsonData.push(Object.keys(jsonData[item]).filter(key=>{
                      return key.toLowerCase().indexOf(text.toLowerCase()) > -1;
                        })
                    )
                }
                filteredJsonData = filteredJsonData.filter(item => {
                    return item.length>0;
                });

                console.log('filteredJsonData', filteredJsonData);

                this.filteredOptions = filteredJsonData;



                // Store data in one property, and filtered in another
                // this.filteredOptions = this.filteredOptions.push(data[0]);
            },
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