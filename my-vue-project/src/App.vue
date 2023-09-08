<template>
  <div class="mainContent__listProduct">
    <h1>Список продуктов</h1>
    <form @submit.prevent="addProduct">
      <input type="text" v-model="newProduct" placeholder="Введите название продукта" required>
      <button type="submit">Добавить</button>
    </form>
    <ul>
      <li class="text" v-for="(product, index) in products" :key="index" :class="{ 'crossedOut': product.crossedOut }">
        {{ product.name }}
        <button @click="crossOutProduct(index)">Вычеркнуть</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data () {
    return {
      products: [],
      newProduct: ''
    }
  },
  methods: {
    addProduct () {
      if (this.newProduct) {
        this.products.push({ name: this.newProduct, crossedOut: false })
        this.newProduct = ''
      }
    },
    crossOutProduct (index) {
      this.products[index].crossedOut = true
      this.products.push(this.products.splice(index, 1)[0])
    }
  },
  mounted () {
    const savedProducts = localStorage.getItem('products')
    if (savedProducts) {
      this.products = JSON.parse(savedProducts)
    } else {
      this.products = [
        { name: 'Продукт 1', crossedOut: false },
        { name: 'Продукт 2', crossedOut: false },
        { name: 'Продукт 3', crossedOut: false }
      ]
      localStorage.setItem('products', JSON.stringify(this.products))
    }
  },
  watch: {
    products: {
      handler (newProducts) {
        localStorage.setItem('products', JSON.stringify(newProducts))
      }
    }
  }
}
</script>
