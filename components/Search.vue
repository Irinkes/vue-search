<template>
    <div class="search" @click="click">
        <search-uz-options v-bind:uzOptions ='uzOptions' @receiveOption="addUzOptionToUrl"/>
        <input type="text" v-model="searchMessage" @change='addSearchMessageToUrl' placeholder="Search goes here"/>
        <router-link :to="{path: submitUrl}" class="btn search-submit">Найти</router-link>

        <div style="padding-top:10px; margin-bottom: 10px;">
            <span v-if="selected">You have selected '{{selected}}'</span>
        </div>

        <vue-autosuggest
                :suggestions="filteredOptions"
                @focus="focusMe"
                @click="clickHandler"
                :on-selected="onSelected"
                :input-props="{id:'autosuggest__input',
                               onInputChange: this.onInputChange,
                               placeholder:'search'}"
        />



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
    #autosuggest__input {
        outline: none;
        position: relative;
        display: block;
        font-family: monospace;
        font-size: 20px;
        border: 1px solid #616161;
        padding: 10px;
        width: 100%;
        box-sizing: border-box;
        -webkit-box-sizing: border-box;
        -moz-box-sizing: border-box;
    }
    #autosuggest__input.autosuggest__input-open {
        border-bottom-left-radius: 0;
        border-bottom-right-radius: 0;
    }

    .autosuggest__results-container {
        position: relative;
        width: 100%;
    }

    .autosuggest__results {
        font-weight: 300;
        margin: 0;
        position: absolute;
        z-index: 10000001;
        width: 100%;
        border: 1px solid #e0e0e0;
        border-bottom-left-radius: 4px;
        border-bottom-right-radius: 4px;
        background: white;
        padding: 0px;
        overflow: scroll;
        max-height: 200px;
    }

    .autosuggest__results ul {
        list-style: none;
        padding-left: 0;
        margin: 0;
    }

    .autosuggest__results .autosuggest__results_item {
        cursor: pointer;
        padding: 15px;
    }

    #autosuggest ul:nth-child(1) > .autosuggest__results_title {
        border-top: none;
    }

    .autosuggest__results .autosuggest__results_title {
        color: gray;
        font-size: 11px;
        margin-left: 0;
        padding: 15px 13px 5px;
        border-top: 1px solid lightgray;
    }

    .autosuggest__results .autosuggest__results_item:active,
    .autosuggest__results .autosuggest__results_item:hover,
    .autosuggest__results .autosuggest__results_item:focus,
    .autosuggest__results .autosuggest__results_item.autosuggest__results_item-highlighted {
        background-color: #ddd;
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
            options: [
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
                searchKey: 'search',
            },
            searchMessage:''
        }),
        mounted() {
            axios
                .get('http://www.json-generator.com/api/json/get/cgviNzYZWq?indent=2')
                .then(response => (this.options[0].data = response.data));
        },

        methods: {
            click: function(){
                // console.log('isClient', this.context);
                // console.log('this.info', this.info);
            },
            onSelected(item) {
                this.selected = item.item;
                let jsonData = this.options[0].data[0];

                for(let dataObj in jsonData) {
                    if(Object.keys(jsonData[dataObj]).includes(item.item)) {
                        this.submitUrlParts[dataObj] = jsonData[dataObj][item.item];
                    }
                }
            },
            clickHandler(item){
                // console.log('item', item);
            },
            // renderSuggestion() {
            //     console.log('suggestion', this.filteredOptions);
            //
            //     // const character = suggestion.item;
            //     return (
            //         '<div style={{ color: "red" }}>{suggestion}</div>;'
            //         );
            // },
            // getSuggestionValue() {
            //     console.log('suggestion111', this.filteredOptions);
            //     // return suggestion.item.name;
            // },
            focusMe(e) {
                // console.log('suggestions', this.suggestions);
            },

            addUzOptionToUrl: function(option)
            {
                this.submitUrlParts.uzOptions = this.uzOptions[option];
                this.submitUrlParts.searchKey = '';
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

                let jsonData = this.options[0].data[0];

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
                // console.log('filteredJsonData', filteredJsonData);

                filteredJsonData = filteredJsonData.filter(item => {
                    return item.length>0;
                });

                if(filteredJsonData.length===0 || ) {
                    console.log('fdfsf');
                }


                for(let i = 0; i < filteredJsonData.length; i++) {
                    filteredJsonData = [].concat(...filteredJsonData);
                }

                this.filteredOptions = [{
                    data: filteredJsonData
                }];

                console.log('data', this.filteredOptions);

                // filteredJsonData = filteredJsonData[0].concat(filteredJsonData[1], filteredJsonData[2]);
                // console.log('filteredJsonData', filteredJsonData);
                //
                // this.filteredOptions = [{
                //     data: filteredJsonData
                // }];



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