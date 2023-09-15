<template>
    <div class="app">
      <h1>Calculadora de Compras</h1>
      <div class="container">
        <div class="input-section">
          <div class="input-box">
            <label for="itemName">Nombre del Artículo:</label>
            <input type="text" v-model="itemName">
          </div>
          <div class="input-box">
            <label for="itemQuantity">Cantidad:</label>
            <input type="number" v-model.number="itemQuantity">
          </div>
          <div class="input-box">
            <label for="itemPrice">Precio Unitario:</label>
            <input type="number" v-model.number="itemPrice">
          </div>
          <button @click="addItem">Agregar Artículo</button>
        </div>
        <div class="product-list">
          <h2>Productos Agregados</h2>
          <table>
            <thead>
              <tr>
                <th>ID</th>
                <th>Nombre del Artículo</th>
                <th>Cantidad</th>
                <th>Precio Unitario</th>
                <th>Total del Producto</th>
                <th>Eliminar</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(product, index) in products" :key="index">
                <td>{{ index + 1 }}</td>
                <td>{{ product.name }}</td>
                <td>{{ product.quantity }}</td>
                <td>{{ product.price }}</td>
                <td>{{ product.quantity * product.price }}</td>
                <td><button @click="removeProduct(index)">Eliminar</button></td>
              </tr>
            </tbody>
          </table>
          <div class="total">
            <p>Total de la Compra: ${{ calculateTotal() }}</p>
            <p>Descuento: ${{ calculateDiscount() }}</p>
            <p>Total a Pagar: ${{ calculateTotalWithDiscount() }}</p>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        itemName: '',
        itemQuantity: 0,
        itemPrice: 0,
        products: [],
      };
    },
    methods: {
      addItem() {
        if (this.itemName && this.itemQuantity > 0 && this.itemPrice > 0) {
          this.products.push({
            name: this.itemName,
            quantity: this.itemQuantity,
            price: this.itemPrice,
          });
  
          this.itemName = '';
          this.itemQuantity = 0;
          this.itemPrice = 0;
        }
      },
      removeProduct(index) {
        this.products.splice(index, 1);
      },
      calculateTotal() {
        let total = 0;
        for (const product of this.products) {
          total += product.quantity * product.price;
        }
        return total.toFixed(2);
      },
      calculateDiscount() {
        let totalQuantity = 0;
        let totalAmount = 0;
        for (const product of this.products) {
          totalQuantity += product.quantity;
          totalAmount += product.quantity * product.price;
        }
        let discount = 0;
        if (totalQuantity >= 12) {
          discount = 0.2; // 20% discount for 12 or more items
        } else if (totalQuantity >= 6) {
          discount = 0.1; // 10% discount for 6 or more items
        } else if (totalAmount >= 240000) {
          discount = 0.15; // 15% discount for total amount >= 240,000
        } else if (totalAmount >= 120000) {
          discount = 0.1; // 10% discount for total amount >= 120,000
        } else if (totalAmount >= 60000) {
          discount = 0.05; // 5% discount for total amount >= 60,000
        }
        return (totalAmount * discount).toFixed(2);
      },
      calculateTotalWithDiscount() {
        const totalAmount = parseFloat(this.calculateTotal());
        const discount = parseFloat(this.calculateDiscount());
        return (totalAmount - discount).toFixed(2);
      },
    },
  };
  </script>
  
  <style scoped>
  .app {
    text-align: center;
    padding: 20px;
  }
  
  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 100%;
    max-width: 800px;
    margin: 0 auto;
  }
  
  .input-section {
    background-color: #f0f0f0;
    padding: 20px;
    border-radius: 5px;
    margin-bottom: 20px;
  }
  
  /* ... Otros estilos ... */
  
  .product-list table {
    width: 100%;
    border-collapse: collapse;
    margin: 0 auto; /* Centra la tabla */
    text-align: left; /* Justifica el texto a la izquierda en la tabla */
  }
  
  /* ... Otros estilos ... */
  </style>
  