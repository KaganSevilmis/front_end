<template>
  <div class="product">
    <strong>{{ product.title }}</strong>
    <p>{{ product.description }}</p>
    <strong>Fiyat : {{ product.price }} | Adet : {{ product.count }}</strong>
    <input type="text" v-model="quantity" placeholder="Enter quantity">
    <br><br>
    <button @click="addToCart">SEPETE EKLE</button>
    <p v-if="product.count === 0" class="out-of-stock">Stokta yok</p>
  </div>
</template>

<script>
export default {
  name: 'ProductItem',
  props: {
    product: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      quantity: 1
    };
  },
  methods: {
    addToCart() {
      if (this.product.count > 0) {
        this.$emit("addToCart", { product: this.product, quantity: this.quantity });
      }
    }
  }
}
</script>

<style scoped>
.product {
  border: 2px dashed #666;
  padding: 5px 10px;
  width: 300px;
  margin-bottom: 5px;
  transition: 1s all;
}
.product:hover {
  border: 2px solid #666;
}
.out-of-stock {
  color: red;
  font-weight: bold;
}
</style>
