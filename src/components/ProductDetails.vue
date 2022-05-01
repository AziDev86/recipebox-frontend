<template>

<div class="container">
<h1> {{productDetails.name}} </h1>
<br>
 {{productDetails.directions}} 
<br>
 {{productDetails.ingredients}} 

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
