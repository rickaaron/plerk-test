<template>
    <div class="item">
        <img :src=" item.image" alt="">
        <p v-text="'Nombre: ' + item.name.esp"></p>
        <p v-text=" 'Precio: ' + '$' +   item.suggested_budget "></p>
        <p> <strong v-text="fnGetCategory(item.category_type)"></strong> </p>
    </div>
</template>

<script>
    export default {
        props: ['item'],

        methods: {
            fnGetCategory(categoryId) {
                switch (categoryId) {
                    case 1:
                        return 'Normal';
                    case 2:
                        return 'Libre';
                    case 3:
                        return 'Personalizada';
                    default:
                        return '-';
                }
            },
        }
    }
</script>

<style>
    .item {
        width: 27.5%;
        margin-bottom: 2.5rem;
        padding: 2rem 0.5rem;
        /* border: 2px solid rgba(0, 0, 0, .2); */
        background-color: white;
        box-shadow: 5px 5px 10px rgba(0, 0, 0, .1);
        border-radius: 10px;
        transition: all 0.5s;
    }

    .item>img {
        width: 100px;
        height: 100px;
        border-radius: 15px;
        background-color: gray;
        object-fit: cover;
        border: 1px solid rgba(255, 255, 255, .1);
        box-shadow: 2px 2px 5px rgba(0, 0, 0, .1);
    }

    @media (max-width: 600px) {
        .item {
            width: 100%;
            padding: 1rem;
            margin-bottom: 1rem;
        }
    }


    @media (min-width: 600px) and (max-width: 900px) {
        .item {
            width: 43%;
        }
    }
</style>