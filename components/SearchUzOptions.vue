<script>
    export default {
        name: 'search-uz-options',
        data: ()=> ({
            uzOptions: [
                'вузы',
                'колледжи',
                'подготовительные курсы',
                'второе высшее',
                'магистратура',
                'аспирантура'
            ],
            showOptionList: false,
            optionLabel: "Вид образования",
            selectedOption: null
        }),
        methods: {
            pickOption: function(option) {
                this.selectedOption = option;
                this.optionLabel = option;
                this.showOption = !this.showOption;

            }
        },
        computed: {
            displayedOptions:  function() {
                return this.selectedOption ? this.uzOptions.filter(option=>option!==this.selectedOption) : this.uzOptions;
            }
        }
    }
</script>

<template>
    <div class="search-options">
        <div v-on:click="showOptionList = !showOptionList" class="option-label">{{ optionLabel }}</div>
        <ul v-show="showOptionList" class="option-list">
            <li v-for="option in displayedOptions" :key="option" class="option" v-on:click="pickOption(option)">
            {{ option }}
            </li>
        </ul>
    </div>
</template>

<style lang="scss">
    .search-options {
        float: left;
        position: relative;
        width: 150px;
        text-align: left;

        .option-list {
            list-style-type: none;
            position: absolute;
            left: 0;
            top: 42px;
            padding-left: 0;
        }

        .option-label {
            margin-right: 20px;
            line-height: 42px;
            height: 42px;
            cursor: pointer;
        }
        .option {
            text-align: left;
            padding: 7px 0;
            border-bottom: 1px solid #ccc;
            cursor: pointer;
        }
    }

</style>