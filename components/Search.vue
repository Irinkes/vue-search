<template>
    <div class="search">
        <search-uz-options />
        <input type="text" placeholder="Search goes here"/>
    </div>
</template>

<style lang="scss">
    .search input{
        width: 300px;
        height: 42px;
        line-height: 42px;
        padding: 10px;
        border: 1px solid #ccc;
    }
</style>
<script>
    import SearchUzOptions from "./SearchUzOptions";
    export default {
        components: {SearchUzOptions}
    }
</script>