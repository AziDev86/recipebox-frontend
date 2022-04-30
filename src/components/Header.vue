<template>
<!-- header component -->
    <header>
        <h1> Recipie Box </h1>
        <Button @toggle-add-product="$emit('toggle-add-product')" text="Add Product"/>
    </header>
</template>

<script>
// reusing button component
import Button from './Button'
export default { 
    name: 'Header',
    components: {
                    Button,
                }
}
</script>

<style  scoped>
    header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 20px;
    }
</style>
