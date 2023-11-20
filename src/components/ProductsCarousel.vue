<template>
  <Carousel :itemsToShow="3" :wrapAround="true" :transition="500" class="carousel">
    <button @click="goToNextSlide">Next</button>
    <Slide v-for="product in products" :key="product.id">
      <div @click="openProduct(product)">
        <img :src="product.Image" class="slide-img-top" alt="Product Image" width="500">
            
        <div class="card">
            <h3 class="slide-header">{{ product.Name }}</h3>
            <p class="pricing">Ksh {{ product.Price - 1}} </p>
        </div>
      </div>
    </Slide>
  </Carousel>
</template>

<script>
import { defineComponent, ref, onMounted } from 'vue';
import { Carousel, Slide } from 'vue3-carousel';
import axios from 'axios';
import 'vue3-carousel/dist/carousel.css';
import { useRouter } from 'vue-router';

export default defineComponent({
  name: 'ProductCarousel',
  components: {
    Carousel,
    Slide,
  },
  setup() {
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
    const router = useRouter();
    let currentSlideIndex = ref(0); // Track the current slide index

    // Fetch products from API
    const fetchProducts = async () => {
      try {
        const response = await axios.get('http://127.0.0.1:5000/api/products'); // Replace 'your-api-endpoint' with the actual API endpoint to fetch products
        products.value = response.data; // Assuming the API response is an array of product objects
      } catch (error) {
        console.error('Failed to fetch products:', error);
      }
    };

     // Function to go to the next slide
    const goToNextSlide = () => {
      currentSlideIndex.value = (currentSlideIndex.value + 1) % 6;
    };

    const openProduct = (product) => {
      // Navigate to the product view page for the clicked product
      // and pass the product ID as a route parameter
      console.log(product.id)
      router.push({ name: 'ProductView', params: { id: product.id } });
    };

    onMounted(() => {
      setInterval(goToNextSlide, 3000);
    });

    return {
      openProduct,
      products,
      goToNextSlide,
    };
  },
});
</script>


<style scoped>
.carousel__slide {
  display: inline-block;
  padding: 0;
}

.carousel__viewport {
  perspective: 2000px;
}

.carousel__track {
  transform-style: preserve-3d;
} 

.carousel__slide--sliding {
  transition: 0.5s;
}

.carousel__slide {
  opacity: 1;
  transform: rotateY(-20deg) scale(0.7);
}

.carousel__slide--active ~ .carousel__slide {
  transform: rotateY(20deg) scale(0.7);
}

.carousel__slide--prev {
  opacity: 1;
  transform: rotateY(-10deg) scale(0.85);
  z-index: 99;
}

.carousel__slide--next {
  opacity: 1;
  transform: rotateY(10deg) scale(0.85);
  z-index: 99;
}

.carousel__slide--active {
  opacity: 1;
  transform: rotateY(0) scale(1);
  z-index: 100;
}
.card {
  text-align: centre;
}

.img.slide-img {
  max-width: 100%;
  max-height: 100%;
}

@media screen and (max-width: 1080px) {
    .carousel__slide , .carousel__slide--prev, .carousel__slide--next, .carousel__slide--active ~ .carousel__slide {
      left: -30%;
      opacity: .0;
    }
    .img.slide-img {
      opacity: .5;
    }
    .carousel__slide--active {
    opacity: 1;
    transform: rotateY(0) scale(1);
    z-index: 100;
    }
    .card {
      text-align: centre;
      width: 500px
    }
}



.pricing {
  font-size: 30px;
  color: rgb(172, 30, 30);
  padding-left: 0;
  font-family: lato;
  font-weight: bold;
}

button {
  position: absolute;
  background-color: rgb(172, 30, 30);
  color: black;
  border: none;
  height: 500px;
  z-index: 600;
}

</style>
