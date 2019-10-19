<template>
    <div class="digui">
        <div v-for="(item,index) in data" :key="index">
                {{item.name}}
                <menuItem :data="item.children" v-if="item.children"/>
        </div>
    </div>
</template>

<script>

export default {
    name:"menuItem",
    props:["data"]
}
</script>

<style scoped>
    .digui div{
        margin-left: 50px;
    }
</style>