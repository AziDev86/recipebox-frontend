<template>

<!-- input form  insert-->
  <form @submit="onSubmit" class="add-form">
    <div class="form-control">
      <label>Product Name</label>
      <input type="text" v-model="name" placeholder="Name" />
    </div>

    <div class="form-control">
      <label>Directions</label>
      <textarea v-model="directions" placeholder="add Directions"></textarea>
    </div>

    <div class="form-control">
      <label>Ingredients</label>
      <textarea v-model="ingredients" placeholder="add Ingredients"></textarea>
    </div>

    <input type="submit" value="Save " class="btn btn-block" />
  </form>
</template>

<script>
export default {
  name: 'AddProduct',

  data() {
    return {    
           name: '',   
           directions: '',
           ingredients: '', 
           }
   },
  methods: {
     onSubmit(e) {
       e.preventDefault()
        //  validation making product name required
       if (!this.name ) {
         alert('Name is required')
        return
      }
      const newProduct = {
        name: this.name,
        directions: this.directions,
        ingredients: this.ingredients,
       }

       this.$emit('add-product', newProduct)
       this.name = ''
       this.directions = ''
       this.ingredients = ''
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