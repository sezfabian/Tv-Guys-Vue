<template>
  <div class="container">
    <Grid :length="products.length" :pageSize="10" :pageProvider="pageProvider" class="grid">
      <template v-slot:placeholder="{ style, index }">
        <div class="item" :style="style">
          <img :src="products[index].Image" alt="https://i.postimg.cc/jdCTJHbh/placeholder-image.png" class="product-image" @click="openProduct(products[index])"/>
          <h2 @click="openProduct(products[index])">{{ products[index].Name }}</h2>
          <div class="product-price">Ksh {{ products[index].Price }}</div>
          <button @click="addToCart(products[index])">Add to Cart</button>
        </div>
      </template>
    </Grid>
  </div>
</template>

<script>
import Grid from "vue-virtual-scroll-grid";
import axios from "axios";
import { useRouter } from 'vue-router';

export default {
  name: "Home",
  components: { Grid },
  data() {
    return {
      products: [],
      router: useRouter()
    };
  },
  methods: {
    // Fetch products from the API
    fetchProducts() {
      axios
        .get("http://127.0.0.1:5000/api/products")
        .then(response => {
          this.products = response.data;
        })
        .catch(error => {
          console.error(error);
        });
    },
    openProduct(product) {
      console.log(product.id)
      this.router.push({ name: 'ProductView', params: { id: product.id } });
    },
    // Return items for the given page
    pageProvider(pageNumber, pageSize) {
      const start = (pageNumber - 1) * pageSize;
      const end = start + pageSize;
      return Promise.resolve(this.products.slice(start, end));
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
    }
  },
  mounted() {
    this.products = [
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
    },
    
	]
  }
};
</script>

<style scoped>
.container {
  padding-left: 30px;
  display: absolute;
  left: -20%;
  flex-direction: column;
  align-items: center;
  z-index: 100;
  background-color: aliceblue;
}

.grid {
  display: grid;
  align-items: center;
  flex-direction: row;
  grid-gap: 10px;
  grid-template-columns: repeat(1, 1fr);
}

@media (min-width: 768px) {
  .grid {
    grid-template-columns: repeat(2, 1fr);
  }
}
@media (min-width: 992px) {
  .grid {
    grid-template-columns: repeat(2, 1fr);
  }
}
@media (min-width: 1280px) {
  .grid {
    grid-template-columns: repeat(2, 1fr);
  }
}
@media (min-width: 1440px) {
  .grid {
    grid-template-columns: repeat(3, 1fr);
  }
}
@media (min-width: 1650px) {
  .grid {
    grid-template-columns: repeat(3, 1fr);
  }
}
@media (min-width: 1890px) {
  .grid {
    grid-template-columns: repeat(4, 1fr);
  }
}
@media (min-width: 2530px) {
  .grid {
    grid-template-columns: repeat(6, 1fr);
  }
}

.item {
  background-color: lightgray;
  padding: 20px 20px;
  text-align: center;
  width: 400px;
  border-radius: 20px;
  cursor: pointer;
  transition: transform 0.3s ease;
}

.item:hover {
  transform: scale(1.2);
}

.product-item {
  background-color: lightgray;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
  text-align: center;
}

.product-image {
  width: 100%;
  max-height: 300px;
  object-fit: contain;
}

.product-details {
  flex-grow: 1;
}

.product-name {
  font-weight: bold;
}

.product-price {
  margin-top: 10px;
}
</style>
