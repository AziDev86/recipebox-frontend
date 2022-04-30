<template>
<!-- reusable button -->
    <button @click="onClick()" class="btn">{{text}}</button>
</template>

<script>
export default {
    name : 'Button',
    props:{
        text: String
        },
    methods:{
        onClick(){
            this.$emit('toggle-add-product')
        }
    }
}
</script>
