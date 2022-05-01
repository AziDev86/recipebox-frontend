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
import Swal from 'sweetalert2'

export default{
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

      getData(status){
         //process.env.VUE_APP_BACKEND_URL
          let products_data = JSON.parse(localStorage.getItem('products_data'))

          if (products_data && !status) {
              // if products_data is not undefined (exists) in the local browser storage, load from local
              this.products = products_data
              }

          else {
          
                this.axios.get('https://recipe-box-wx8wd.ondigitalocean.app/api/products/')
                .then((result)=>{
                  this.products= result.data

                  //store the retrived data in local storage
                  localStorage.setItem('products_data', JSON.stringify(result.data));

                })
                .catch(error => console.log(error))
                .finally(() => this.loading = false)
              }
      },

      deleteProduct(id)
      {
          if(confirm("Are you sure you want to delete this product"))
          {
              this.axios.delete('https://recipe-box-wx8wd.ondigitalocean.app/api/products/'+id)
              .then((result)=>{
                  var status =1;
                  this.getData(status);
                  this.productDetails='';
                  this.showProductDetails = false;

                  // giving user feedback
                  Swal.fire({
                    title: "Success!",
                    text:   "Product has been deleted",
                    icon: 'success', 
                    });
                  })
              .catch(function (error) {
              // giving user feedback
              Swal.fire({
                        title: "OPPS! could't delete product",
                        text:   "Something went wrong, make sure you are connected to the internet",
                        icon: 'warning',
                    });
            });
           }
          },

      selectProduct(id){
        this.axios.get('https://recipe-box-wx8wd.ondigitalocean.app/api/products/'+id)
        .then((result)=>{
              this.showProductDetails = true;
              this.showProductUpdateForm =false;
              this.productDetails= result.data
           })
        .catch(error => console.log(error))
        .finally(() => this.loading = false)

        },



      editProductDetails(id){
        this.showProductDetails =false;
        this.showProductUpdateForm =true;

        this.axios.get(process.env.VUE_APP_BACKEND_URL+id)
        .then((result)=>{
        this.productDetails= result.data
           })            
           .catch(function (error) {
            // giving user feedback
              Swal.fire({
                        title: "OPPS! could't load product",
                        text:   "Something went wrong, make sure you are connected to the internet",
                        icon: 'warning',
                    });

              console.log(error);
            });
        
          },
          
      updateProduct(product){
        this.axios.put('https://recipe-box-wx8wd.ondigitalocean.app/api/products/'+product.id,{
          name: product.name,
          directions: product.directions,
          ingredients: product.ingredients
        })
              
        .then((result)=>{
          var status =1;
          this.getData(status);
          // giving user feedback
          Swal.fire({
                      title: "Success!",
                      text:   "Product has been updated",
                      icon: 'success',
                      
                    });
              })
            .catch(function (error) {
              Swal.fire({
                        title: "OPPS! could't update product",
                        text:   "Something went wrong, make sure you are connected to the internet",
                        icon: 'warning',
                      
                    });

            });
                        this.showProductUpdateForm =false;
                        this.showProductDetails =false;

              },
        addProduct(product){
            this.axios.post('https://recipe-box-wx8wd.ondigitalocean.app/api/products/', {
            name: product.name,
            directions: product.directions,
            ingredients: product.ingredients
            })           
            .then((result)=>{
              var status =1;
              this.getData(status);

                  Swal.fire({
                  title: "Success!",
                  text:   "Product has been added",
                  icon: 'success',
                
                   });
    

              }).catch(function (error) {
                    Swal.fire({
                        title: "OPPS! could't add product",
                        text:   "Something went wrong, make sure you are connected to the internet",
                        icon: 'warning',
                      
                    });
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