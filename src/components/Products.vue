<template>
<!-- displayproducts list component -->
  <div class="container">
          <h3> list of Recipies </h3>
          <table>

                <th>Recipe Name</th>
                <th>View</th>
                <th>Edit</th>
                <th>Delete</th>
                <!-- itteating through products array -->
                <tr v-for="product in products" :key="product.id"  class="product">
                    <td> {{product.name}} </td>
                    <td> <a href="#"> <i @click="onSelect(product.id)" class="fas fa-eye "> </i> </a> </td>
                    <td> <a href="#"><i @click="onEdit(product.id)" class="fas fa-pen "></i></a> </td>
                    <td> <a href="#"> <i @click="onDelete(product.id)" class="fas fa-times"> </i> </a> </td>
                </tr>
                    
        </table>
  </div>   
</template>
 
<script>
    export default{  
    name: 'Products',
    props: {
        products: Array  
    },

    emits: ['delete-product', 'select-product', 'edit-product' ],  

    methods: {
        onDelete(id){
            this.$emit('delete-product',id)   
            },
              onSelect(id){
            this.$emit('select-product',id)   
            },
                onEdit(id){
            this.$emit('edit-product',id)   
            }
    }
 
}
</script>

<style scoped>
  .fas {
      margin: 2px;
      padding: 0px;
      text-align: right;
  }
  .product{
  background : #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
  }
  .product h3{
      display:flex;
      align-items: center;
      justify-content: space-between;
  }
  table {
    font-family: arial, sans-serif;
    border-collapse: collapse;
    width: 100%;
  }

  td, th {
    border: 1px solid #dddddd;
    text-align: left;
    padding: 8px;
  }

  tr:nth-child(even) {
    background-color: #dddddd;
  }
</style>