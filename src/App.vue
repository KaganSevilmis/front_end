<template>
  <div class="container">
    <div class="main-content">
      <div class="cart">
        <h3>SEPET</h3>
        <ul>
          <li v-for="item in carItems" :key="item.product.id">
            {{ item.product.title }} - Adet: {{ item.quantity }}
            <button @click="removeFromCart(item.product)">Kaldır</button>
          </li>
        </ul>
      </div>

      <div class="product-list">
        <ProductItem 
          v-for="product in productList" 
          :key="product.id" 
          :product="product" 
          @addToCart="addToCart"
        />
      </div>
    </div>

    <div class="sidebar">
      <h3>Diğer Ürünler</h3>
      <div v-for="product in productList" :key="product.id" class="other-product">
        <ProductItem 
          :product="product" 
          @addToCart="addToCart"
        />
      </div>
    </div>

    <input type="text" writingsuggestions="true" placeholder="Search products">
  </div>
</template>

<script>
import ProductItem from "@/components/product.vue";

export default {
  components: {
    ProductItem
  },
  data() {
    return {
      productList: [
        {
          id: 1,
          title: "MacBook Pro",
          price: 100,
          count: 0,
          description: "Açıklama 1"
        },
        {
          id: 2,
          title: "Iphone",
          price: 50,
          count: 100,
          description: "Açıklama 2"
        },
        {
          id: 3,
          title: "Klavye",
          price: 10,
          count: 1000,
          description: "Açıklama 3"
        }
      ],
      carItems: []
    };
  },
  methods: {
    addToCart({ product, quantity }) {
      const existingItem = this.carItems.find(item => item.product.id === product.id);
      if (existingItem) {
        existingItem.quantity += quantity;
      } else {
        this.carItems.push({ product, quantity });
      }
      // Update the product count in the productList
      const productToUpdate = this.productList.find(p => p.id === product.id);
      if (productToUpdate) {
        productToUpdate.count -= quantity;
      }
    },
    removeFromCart(product) {
      const itemIndex = this.carItems.findIndex(item => item.product.id === product.id);
      if (itemIndex !== -1) {
        const item = this.carItems[itemIndex];
        // Update the product count in the productList
        const productToUpdate = this.productList.find(p => p.id === product.id);
        if (productToUpdate) {
          productToUpdate.count += item.quantity;
        }
        // Remove the item from the cart
        this.carItems.splice(itemIndex, 1);
      }
    }
  }
}
</script>

<style scoped>
.container {
  display: flex;
}
.main-content {
  flex: 3;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.cart {
  margin-bottom: 50px;
}
.product-list {
  margin-bottom: 20px;
}
.sidebar {
  flex: 1;
  padding-left: 20px;
  border-left: 1px solid #ddd;
  background-color: #ffebcd; /* Turuncu arkaplan rengi */
  padding: 10px;
  border-radius: 5px;
}
.sidebar h3 {
  color: #d35400; /* Daha koyu turuncu başlık rengi */
}
.other-product {
  margin-bottom: 10px;
}
</style>
