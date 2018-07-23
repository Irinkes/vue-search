<template>
    <section class="container">
        <div>
            Результаты поиска по колледжам
        </div>
    </section>
</template>

<script>

    export default {

        validate ({ params }) {
            // Должен быть строкой
            if(typeof params.id === 'string') {
                return true;
            } else {
                return false;
            }

        }
    }
</script>

<style lang="scss">
    @import "~/css/main.scss";
</style>
