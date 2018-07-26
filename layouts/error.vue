<template>
    <div class="container">
        <h1 v-if="error.statusCode === 404">Страница не найдена</h1>
        <div v-else>
            <h1>{{ error.statusCode }}</h1>
            <h2 class="error-message">{{ error.message }}</h2>
        </div>

        <nuxt-link to="/">Вернуться на главную</nuxt-link>
    </div>
</template>

<script>
    export default {
        name: 'nuxt-error',
        props: ['error']
    }
</script>
<style lang="scss">
    .error-message {
        font-size: 14px;
        font-weight: normal;
        margin: 0;
        padding: 0;
        color: red;
    }
</style>