<template>
  <div class="panel-product">
    <div class="left-panel">
      <ul class="menu-list">
        <li @click="currentAction = 'create'; clearSelectedProduct()">Crear Producto</li>
        <li @click="currentAction = 'update'; clearSelectedProduct()">Actualizar Producto</li>
        <li @click="currentAction = 'delete'; clearSelectedProduct()">Eliminar Producto</li>
      </ul>
    </div>

    <div class="right-panel">
      <!-- Seccion Crear Producto -->
      <div v-if="currentAction == 'create'">
        <h2>Crear nuevo producto</h2>

        <form action="">
          <div>
            <label for="productCategory">Categoria del producto</label>
            <select name="productCategory" id="productCategory">
              <option value="">Selecciona una categoria del producto</option>
              <option value="">Categoria 1</option>
              <option value="">Categoria 2</option>
              <option value="">Categoria 3</option>
            </select>
          </div>

          <div>
            <label for="productImage">Imagen</label>
            <input type="file" placeholder="Imagen del producto">
          </div>
          <div>
            <label for="productName">Nombre</label>
            <input type="text" placeholder="Nombre del producto">
          </div>
          <div>
            <label for="productPrice">Precio</label>
            <input type="text" placeholder="Precio del producto">
          </div>
          <div>
            <label for="productStock">Stock</label>
            <input type="text" placeholder="Stock del producto">
          </div>
          <div>
            <label for="productDescription">Descripción</label>
            <textarea name="productDescription" id="productDescription" cols="30" rows="10"></textarea>
          </div>
          <button type="submit">Crear</button>
        </form>
      </div>

      <!-- Seccion Actualizar Producto -->
      <div v-else-if="currentAction == 'update'">
        <h2>Actualizar producto</h2>

        <div>
            <label for="productSearch">Filtro de búsqueda</label>
            <input 
              type="text" 
              v-model="searchFilter" 
              placeholder="Buscar productos..." 
              @input="filterProducts"
            >
        </div>
        <div>
          <div v-for="product in filteredProducts" :key="product.id">
            <p>{{ product.name }}</p>
            <button @click="selectProductToUpdate(product)">Seleccionar</button>
          </div>
        </div>

        <form v-if="selectedProduct" @submit.prevent="updateProduct">
          <div>
            <label for="productCategory">Categoria del producto</label>
            <select v-model="selectedProduct.category" name="productCategory" id="productCategory">
              <option value="categoria1">Categoria 1</option>
              <option value="categoria2">Categoria 2</option>
              <option value="categoria3">Categoria 3</option>
            </select>
          </div>

          <div>
            <label for="productImage">Imagen</label>
            <input type="file" placeholder="Imagen del producto">
          </div>
          <div>
            <label for="productName">Nombre</label>
            <input v-model="selectedProduct.name" type="text" placeholder="Nombre del producto">
          </div>
          <div>
            <label for="productPrice">Precio</label>
            <input v-model="selectedProduct.price" type="text" placeholder="Precio del producto">
          </div>
          <div>
            <label for="productStock">Stock</label>
            <input v-model="selectedProduct.stock" type="text" placeholder="Stock del producto">
          </div>
          <div>
            <label for="productDescription">Descripción</label>
            <textarea v-model="selectedProduct.description" name="productDescription" id="productDescription" cols="30" rows="10"></textarea>
          </div>
          <button type="submit">Actualizar</button>
        </form>
      </div>

      <!-- Seccion Eliminar Producto -->
      <div v-else-if="currentAction == 'delete'">
        <h2>Eliminar producto</h2>
        <div>
          <div v-for="product in filteredProducts" :key="product.id" class="product-item">
            <p>{{ product.name }}</p>
            <button @click="confirmDelete(product)">Eliminar</button>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  name: 'PanelProdcut',
  data() {
    return {
      currentAction: null,
      formData: {
        productCategory: '',
        productImage: '',
        productName: '',
        productPrice: '',
        productStock: '',
        productDescription: '',
      },
      products: [
        {
          id: 1,
          category: 'Categoria 1',
          image: '',
          name: 'Producto 1',
          price: 100,
          stock: 10,
          description: 'Descripción del producto 1',
        },
        {
          id: 2,
          category: 'Categoria 2',
          image: '',
          name: 'Producto 2',
          price: 200,
          stock: 20,
          description: 'Descripción del producto 2',
        },
        {
          id: 3,
          category: 'Categoria 3',
          image: '',
          name: 'Producto 3',
          price: 300,
          stock: 30,
          description: 'Descripción del producto 3',
        }
      ],
      searchFilter: '',
      filteredProducts: [],
    }
  },
  methods: {
    clearSelectedProduct() {
      this.selectedProduct = null;
      this.searchFilter = '';
      this.filteredProducts = null;
    },
    filterProducts() {
      if (!this.searchFilter) {
        this.filteredProducts = this.products;
        return;
      }
      const searchTerm = this.searchFilter.toLowerCase();
      this.filteredProducts = this.products.filter(product => 
        product.name.toLowerCase().includes(searchTerm) ||
        product.category.toLowerCase().includes(searchTerm)
      );
    },
    selectProductToUpdate(product) {
      this.clearSelectedProduct();
      this.selectedProduct = { ...product };
      if (this.currentAction == 'update') 
        this.currentAction = 'update'
    },
    updateProduct() {
      const index = this.products.findIndex(p => p.id === this.selectedProduct.id);
      if (index !== -1) {
       this.products[index] = this.selectedProduct;
       this.selectedProduct = null;
      }
    },
    confirmDelete(product) {
      if (confirm(`¿Estás seguro de eliminar el producto ${product.name}?`)) {
        this.products = this.products.filter(p => p.id !== product.id);
      }
    },
  },
}
</script>

<style scoped>
.panel-product {
  display: flex;
  height: 100vh;
  width: 100%;
}

.left-panel {
  width: 250px;
  padding: 20px;
}

.right-panel {
  flex: 1;
  padding: 20px;
}

.menu-list {
  list-style: none;
  padding: 0;
}

.menu-list li {
  padding: 10px;
  margin: 5px 0;
  cursor: pointer;
  border-radius: 4px;
}

.menu-list li:hover {
  background-color: #e0e0e0;
}

</style>

