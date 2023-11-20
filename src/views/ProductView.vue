<template>
  <div class="product-view">
    <div class="product-image">
      <img :src="product.Image" alt="Product Image" />
    </div>
        <div class="product-details">
            <h2>{{ product.Name }}</h2>
            <h3 class="price">Ksh {{ product.Price }}.00</h3>
            <button @click="addToCart(product)" v-if="!inCart">Add to Cart</button>
            <button @click="removeFromCart" v-if="inCart">Remove from Cart</button>
        </div>
        <div class="buttons">
            <p>{{ product.Description }}</p>
            <p>Warranty: {{ product.Warranty }} Months</p>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'ProductView',
  data() {
    return {
      product: {},
      editedProduct: {},
      inCart: false,
      editMode: false
    };
  },
  created() {
    const productId = this.$route.params.id;
    this.fetchProduct(productId);
  },
  methods: {
    fetchProduct(productId) {
    const products = [
    {
        "id": 0,
        "Name": "Smart LED TV 55\"",
        "Description": "Ultra HD Smart LED TV with built-in streaming services.",
        "Cost": 2000,
        "Price": 2799,
        "Brand": "TechVision",
        "Size": "43 inches",
        "Os": "Android",
        "Dimensions": "48 x 28 x 2 inches",
        "Resolution": "4K",
        "Warranty": "2 years",
        "Design": "Slim Bezel",
        "Sub_cat_id": 1,
        "Category_id": 1,
        "Image": "https://i.postimg.cc/hGbmrHPK/Hisense-43-Inch.png",
        "Stock": 30
    },
    {
        "id": 1,
        "Name": "Smart LED TV 55\"",
        "Description": "Ultra HD Smart LED TV with built-in streaming services.",
        "Cost": 2000,
        "Price": 2799,
        "Brand": "TechVision",
        "Size": "43 inches",
        "Os": "Android",
        "Dimensions": "48 x 28 x 2 inches",
        "Resolution": "4K",
        "Warranty": "2 years",
        "Design": "Slim Bezel",
        "Sub_cat_id": 1,
        "Category_id": 1,
        "Image": "https://i.postimg.cc/hGbmrHPK/Hisense-43-Inch.png",
        "Stock": 30
    },
    {
        "id": 2,
        "Name": "QLED TV 65\"",
        "Description": "Quantum Dot QLED TV with immersive display technology.",
        "Cost": 71200,
        "Price": 81499,
        "Brand": "QuantumVision",
        "Size": "65 inches",
        "Os": "Tizen",
        "Dimensions": "55 x 32 x 2.5 inches",
        "Resolution": "8K",
        "Warranty": "3 years",
        "Design": "Edge-to-Edge Screen",
        "Sub_cat_id": 1,
        "Category_id": 1,
        "Image": "https://i.postimg.cc/yNMBg9j3/synix-50.png",
        "Stock": 20
    },
    {
        "id": 3,
        "Name": "OLED TV 50\"",
        "Description": "Premium OLED TV with deep blacks and vibrant colors.",
        "Cost": 5900,
        "Price": 65199,
        "Brand": "Luxor",
        "Size": "50 inches",
        "Os": "WebOS",
        "Dimensions": "45 x 25 x 1.5 inches",
        "Resolution": "4K",
        "Warranty": "2.5 years",
        "Design": "Ultra-Thin Profile",
        "Sub_cat_id": 1,
        "Category_id": 1,
        "Image": "https://i.postimg.cc/cCmRMrhd/Vision-43.png",
        "Stock": 25
    },
    {
        "id": 4,
        "Name": "ULED TV 50\"",
        "Description": "Premium OLED TV with deep blacks and vibrant colors.",
        "Cost": 5900,
        "Price": 65199,
        "Brand": "Luxor",
        "Size": "50 inches",
        "Os": "WebOS",
        "Dimensions": "45 x 25 x 1.5 inches",
        "Resolution": "4K",
        "Warranty": "2.5 years",
        "Design": "Ultra-Thin Profile",
        "Sub_cat_id": 1,
        "Category_id": 1,
        "Image": "https://i.postimg.cc/447mqxCd/skyworth-50.png",
        "Stock": 25
    },
    {
        "id": 5,
        "Name": "Galaxy OLED TV 50\"",
        "Description": "Premium OLED TV with deep blacks and vibrant colors.",
        "Cost": 5900,
        "Price": 65199,
        "Brand": "Luxor",
        "Size": "50 inches",
        "Os": "WebOS",
        "Dimensions": "45 x 25 x 1.5 inches",
        "Resolution": "4K",
        "Warranty": "2.5 years",
        "Design": "Ultra-Thin Profile",
        "Sub_cat_id": 1,
        "Category_id": 1,
        "Image": "https://i.postimg.cc/cCmRMrhd/Vision-43.png",
        "Stock": 25
    },
    {
        "id": 6,
        "Name": "LCD TV 50\"",
        "Description": "Premium OLED TV with deep blacks and vibrant colors.",
        "Cost": 5900,
        "Price": 65199,
        "Brand": "Luxor",
        "Size": "50 inches",
        "Os": "WebOS",
        "Dimensions": "45 x 25 x 1.5 inches",
        "Resolution": "4K",
        "Warranty": "2.5 years",
        "Design": "Ultra-Thin Profile",
        "Sub_cat_id": 1,
        "Category_id": 1,
        "Image": "https://i.postimg.cc/447mqxCd/skyworth-50.png",
        "Stock": 25
    }
	];
    
    let foundProduct = null;

  // Loop through the products array
  for (let i = 0; i < products.length; i++) {
    // Check if the current product has the specified ID
    if (products[i].id === productId) {
      // Assign the current product to foundProduct
      foundProduct = products[i];
      break; // Exit the loop once the product is found
    }
  }

  // Check if a product was found
  if (foundProduct) {
    // Update the 'product' data property with the found product
    this.product = foundProduct;
    console.log(this.product.Name);
  } else {
    console.error(`Product with ID ${productId} not found.`);
   this.product = products[productId]
  }
},

    // Add product to cart
    addToCart(product) {
      const existingItem = this.$store.state.cartItems.find(item => item.id === product.id);
      if (existingItem) {
        // Item already exists in cart, increment quantity
        this.$store.commit('incrementCartItemQuantity', existingItem.id);
      } else {
        // Item doesn't exist in cart, add as new item
        const cartItem = {
          id: product.id,
          name: product.Name,
          price: product.Price,
          quantity: 1
        };
        this.$store.commit('addToCart', cartItem);
    }
    },
    removeFromCart() {
      this.$store.commit('decrementCartItemQuantity', item.id);
    }
  }
};
</script>

<style>
.product-view {
  position: relative;
  top: 120px;
  padding: 100px 20px;
  display: inline-block;
  flex-direction: column;
  background: white;
  border-radius: 20px;
  width : 60vw;
}


.product-view img {
  width: 50vw;
}

</style>
