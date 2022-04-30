<template>

<div class="container">
<h1> {{productDetails.name}} </h1>
<h4> {{productDetails.directions}} </h4>
<h4> {{productDetails.ingredients}} </h4>

</div>
</template>
<script>

export default ({
name: 'ProductDetails',


props:{  
         productDetails: Array
     },

     emits: ['delete-product'],  
 
methods: {
        onDelete(id){
        this.$emit('delete-product',id)   
        }
}

})
</script>
