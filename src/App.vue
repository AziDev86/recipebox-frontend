<template>
<div class="container">

  <Header @toggle-add-product="toggleAddProduct"/>
  <div v-show="showAddProduct">
      <AddProduct  @add-product="addProduct"/>
  </div>
  <Products  @edit-product="editProductDetails" @delete-product="deleteProduct" @select-product="selectProduct" :products="products" />

    <div v-show="showProductDetails">
   <ProductDetails  :productDetails="productDetails"/> 
   </div>
       <div v-show="showProductUpdateForm">
      <EditProduct  @update-product="updateProduct" :productDetails="productDetails"/> 
    </div>
</div>
  
</template>

<script>
import  Header from './components/Header'
import  Products from './components/Products'
import  AddProduct from './components/AddProduct'
import  ProductDetails from './components/ProductDetails'
import EditProduct from './components/EditProduct.vue'


export default{

      data(){
      products:[]
      

    },
    name: 'App',
    components:{
      Header,
      Products,
      ProductDetails,
      AddProduct,
      EditProduct
    },
  data(){
      return{
            products:[],
            productDetails:[],

            showAddProduct: false,
            showProductDetails: false,
            showProductUpdateForm: false, 
        } 
    },
    methods: {
      toggleAddProduct(){
        this.showAddProduct = !this.showAddProduct
      },

      getData(){

            this.axios.get('http://127.0.0.1:8000/api/products')
            .then((result)=>{
              console.warn(result)
              this.products= result.data
            })
      },

      deleteProduct(id)
      {
          if(confirm("Are you sure you want to delete this product"))
          {
              this.axios.delete("http://127.0.0.1:8000/api/products/"+id)
              .then((result)=>{
                this.getData();
                  this.productDetails='';
                  this.showProductDetails = false;

              })
           }
          },
      selectProduct(id)
      {

           this.axios.get("http://127.0.0.1:8000/api/products/"+id)
           .then((result)=>{
                      this.showProductDetails = true;
                             this.showProductUpdateForm =false;
           this.productDetails= result.data
           })
        
          },

          editProductDetails(id)
          {
                          this.showProductDetails =false;
                          this.showProductUpdateForm =true;

           this.axios.get("http://127.0.0.1:8000/api/products/"+id)
           .then((result)=>{
           this.productDetails= result.data
           })
        
          },
          
          updateProduct(product)
          {
           this.axios.put("http://127.0.0.1:8000/api/products/"+product.id,
              {
                name: product.name,
                directions: product.directions,
                ingredients: product.ingredients
              })
              .then((result)=>{
                this.getData();
              })
            .catch(function (error) {
              console.log(error);
            });
                          this.showProductUpdateForm =false;
                          this.showProductDetails =false;

              },
        addProduct(product)
        {
              this.axios.post('http://127.0.0.1:8000/api/products', {
              name: product.name,
              directions: product.directions,
              ingredients: product.ingredients
            })
              .then((result)=>{
                this.getData();
              })
            .catch(function (error) {
              console.log(error);
            });
        }
    },

   
     mounted() {
       this.getData()


     }

}

</script>


<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: 'Poppins', sans-serif;
}
.container {
  max-width: 800px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
</style>