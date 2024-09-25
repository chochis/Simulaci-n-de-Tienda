<template>
    <div class="container my-4">
        <!-- Título de la tienda con ícono -->
    <div class="d-flex align-items-center justify-content-center mb-4">
        <!-- Icono de Bootstrap o Font Awesome -->
        <i class="bi bi-shop-window me-2" style="font-size: 2rem;"></i> <!-- Bootstrap Icon -->
        <!-- <i class="fas fa-store me-2" style="font-size: 2rem;"></i> --> <!-- Font Awesome Icon (alternativa) -->
        <h1 class="display-4 fw-bold">Mi tienda Virtual</h1>
    </div>
    <!-- Mensaje de carga -->
    <div v-if="loading" class="text-center">
        <div class="spinner-border" role="status">
        <span class="visually-hidden">Cargando...</span>
        </div>
    </div>
    <!-- Input de búsqueda -->
    <div class="input-group mb-4" v-if="!loading">
    <input
        v-model="searchQuery"
        @input="filterProducts"
        type="text"
        class="form-control"
        placeholder="Buscar productos"
        aria-label="Buscar productos"
    />
    </div>
    <!-- Grilla de productos -->
    <div class="row g-4">
    <!-- Producto individual -->
    <div
        v-for="product in filteredProducts"
        :key="product.id"
        class="col-12 col-sm-6 col-md-4 col-lg-3 d-flex align-items-stretch"
    >
        <div class="card h-100 shadow-sm">
        <!-- Imagen del producto -->
        <img :src="product.image" class="card-img-top img-fluid" alt="Product image" />
        <!-- Detalles del producto -->
        <div class="card-body d-flex flex-column">
            <h5 class="card-title">{{ product.title }}</h5>
            <p class="card-text text-muted">$ {{ product.price }}</p>
            <button @click="viewProductDetails(product.id)" class="btn btn-outline-primary mt-auto">Ver Detalles</button>
        </div>
        </div>
    </div>
    </div>
    <!-- Mensaje si no hay productos -->
    <div v-if="filteredProducts.length === 0 && !loading" class="text-center">
        <p>No se encontraron productos.</p>
    </div>
<!-- Botón "Ver más" -->
<div class="d-flex justify-content-center mt-4">
    <button v-if="!allLoaded" @click="loadMore" class="btn btn-outline-primary">Ver más</button>
    </div>
    <!-- Modal para detalles del producto -->
    <div v-if="selectedProduct" class="modal fade show" tabindex="-1" style="display: block;" aria-modal="true">
    <div class="modal-dialog modal-lg"> <!-- Cambiado a modal-lg para hacerlo más amplio -->
        <div class="modal-content">
        <div class="modal-header">
            <h5 class="modal-title">{{ selectedProduct.title }}</h5>
            <button type="button" class="btn-close" @click="closeModal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
            <div class="row">
            <!-- Imagen del producto -->
            <div class="col-12 col-md-6">
                <img :src="selectedProduct.image" class="img-fluid" alt="Product image" />
            </div>
            <!-- Detalles del producto -->
            <div class="col-12 col-md-6">
                <p><strong>Descripción:</strong></p>
                <p class="text-muted">{{ selectedProduct.description }}</p>
                <p><strong>Precio:</strong> ${{ selectedProduct.price }}</p>
                <button class="btn btn-primary mt-3">Añadir al carrito</button>
            </div>
            </div>
        </div>
        <div class="modal-footer">
            <button type="button" class="btn btn-secondary" @click="closeModal">Cerrar</button>
        </div>
        </div>
    </div>
    </div>
    </div>
</template>

<script>
export default {
    data() {
    return {
        products: [],
        filteredProducts: [],
        searchQuery: "",
        currentPage: 1,
        itemsPerPage: 8,
        allLoaded: false,
        selectedProduct: null,
        loading: true, // Estado de carga
    };
    },
    methods: {
    async fetchProducts() {
        this.loading = true; // Activar el estado de carga
        try {
        const response = await fetch(`https://fakestoreapi.com/products`);
        if (!response.ok) throw new Error("Error al cargar los productos");
        const data = await response.json();
        this.products = data;
        this.filteredProducts = this.products.slice(0, this.itemsPerPage);
        } catch (error) {
        console.error("Error fetching products:", error);
        alert("No se pudieron cargar los productos. Intenta de nuevo más tarde.");
        } finally {
          this.loading = false; // Desactivar el estado de carga
        }
    },
    filterProducts() {
        const query = this.searchQuery.toLowerCase();
        this.filteredProducts = this.products
        .filter((product) =>
            product.title.toLowerCase().includes(query)
        )
          .slice(0, this.itemsPerPage * this.currentPage);
    },
    loadMore() {
        this.currentPage++;
        const nextProducts = this.products.slice(0, this.itemsPerPage * this.currentPage);
        this.filteredProducts = nextProducts;
        if (nextProducts.length >= this.products.length) {
        this.allLoaded = true;
        }
    },
    async viewProductDetails(id) {
        try {
        const response = await fetch(`https://fakestoreapi.com/products/${id}`);
        if (!response.ok) throw new Error("Error al cargar el producto");
        this.selectedProduct = await response.json();
        } catch (error) {
        console.error("Error fetching product details:", error);
        alert("No se pudo cargar el detalle del producto. Intenta de nuevo más tarde.");
        }
    },
    closeModal() {
        this.selectedProduct = null;
    },
    },
    mounted() {
    this.fetchProducts();
    },
};
</script>
<style scoped>

/* Estilos adicionales para el título */
h1 {
    color: white; /* Color del texto */
    text-shadow:
    -1px -1px 0 black,
    1px -1px 0 black,
    -1px  1px 0 black,
    1px  1px 0 black;
}

.bi-shop-window, .fas {
  color: #e5e5e6; /* Color primario */
}
/* Tamaño y ajustes de la imagen */
.card-img-top {
height: 200px;
  object-fit: contain; /* Asegura que la imagen se ajuste sin deformarse */
}

/* Espaciado uniforme entre las tarjetas */
.card {
border-radius: 10px;
transition: transform 0.3s ease;
}

.card:hover {
  transform: translateY(-12px); /* Efecto de elevación en hover */
}

/* Agregar sombra a las tarjetas */
.card-body {
display: flex;
flex-direction: column;
justify-content: space-between;
}

.g-4 > * {
padding: 1rem;
}
/* Estilos del modal */
.modal-dialog {
  max-width: 800px; /* Hace el modal más ancho */
}

.modal-body {
  max-height: 70vh; /* Limita la altura del cuerpo del modal */
  overflow-y: auto; /* Permite hacer scroll si el contenido es muy largo */
}

.modal-content {
  border-radius: 10px; /* Bordes redondeados */
  box-shadow: 0px 4px 20px rgba(0, 0, 0, 0.1); /* Sombra suave */
}

/* Estilo de la imagen dentro del modal */
.modal-body img {
    object-fit: cover; /* Asegura que la imagen mantenga su proporción */
    border-radius: 5px;
}

/* Estilos de transición */
.modal.fade {
  transition: opacity 0.3s ease-in-out; /* Transición suave para el modal */
}

.modal-header, .modal-footer {
  background-color: #f8f9fa; /* Colores más suaves */
}

.modal-title {
font-size: 1.5rem;
font-weight: bold;
color: #343a40;
}

.modal-body p {
margin-bottom: 0.5rem;
}

.btn-primary {
background-color: #007bff;
font-family: 'Montserrat', sans-serif;
font-weight: 600;
}

.btn-secondary {
background-color: #6c757d;
}

</style>