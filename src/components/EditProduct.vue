<template>
<!-- input form  update-->

  <form @submit="onSubmit" class="add-form">
    <div class="form-control">
      <label>Product Name</label>
      <input type="text" v-model="productDetails.name" placeholder="Name" />
    </div>
     
    <div class="form-control">
      <label>Directions</label>
      <textarea v-model="productDetails.directions" placeholder="add Directions"> </textarea>
    </div>

    <div class="form-control">
      <label>Ingredients</label>
      <textarea v-model="productDetails.ingredients" placeholder="add Ingredients"> </textarea>
    </div>

    <input type="submit" value="Update" class="btn btn-block" />
  </form>
</template>

<script>
export default {
  name: 'updatedProduct',

 data() {
    return {    
           name: '',   
           directions: '',
           ingredients: '', 
          }
 
   },

  props:{  
         productDetails: Array
        },

   methods: {
     onSubmit(e) {
       e.preventDefault()
      //  validation making product name required
       if (!this.productDetails.name ) {
         alert('Name is required')
        return
      }
       const updateProduct = {
           id: this.productDetails.id,
          name: this.productDetails.name,
          directions: this.productDetails.directions,
          ingredients: this.productDetails.ingredients,
       }

       this.$emit('update-product', updateProduct)
       this.productDetails.name = ''
       this.productDetails.directions = ''
       this.productDetails.ingredients = ''
     },
   },
}


</script>

<style scoped>
  .add-form {
    margin-bottom: 40px;
  }
  .form-control {
    margin: 20px 0;
  }
  .form-control label {
    display: block;
  }
  .form-control input  {
    width: 100%;
    height: 40px;
    margin: 5px;
    padding: 3px 7px;
    font-size: 17px;
  }

  .form-control textarea  {
    width: 100%;
    min-width: 100%;
    height: 40px;
    margin: 5px;
    padding: 3px 7px;
    font-size: 17px;
    max-width: 100%;
    min-height: 100px;
  }
</style>