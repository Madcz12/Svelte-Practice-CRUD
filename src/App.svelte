<script>
  
  import {v4} from 'uuid'
  import Noty from 'noty'
  import Toastify from 'toastify-js'
  import "toastify-js/src/toastify.css" 

  import 'noty/lib/noty.css';
  import 'noty/lib/themes/sunset.css'

  let products = [
    {
      id: 1,
      name: 'Alienware Nova GT',
      description: 'Dell Laptop',
      category: 'laptop'
    },
    {
      id: 2,
      name: 'Logitech G Pro',
      description: 'Gaming Mouse',
      category: 'peripherals'
    },  
  ];

  let product = {
    id: "",
    name: "",
    description: "",
    category: "",
    imageURL: "",
  };

  // variable para almacenar el estado al dar click
  let editStatus = false;  


  const cleanProduct = () => {
    product = {
      id: '',
      name: "",
      description: "",
      category: "",
      imageURL: "",
    }
  }

  const addProduct = () => {
    
    const newProduct = {
      id: v4(),
      name: product.name,
      description: product.description,
      category: product.category,
      imageURL: product.imageURL
    }
    products = products.concat(newProduct)
    cleanProduct();
    Toastify({
    text: "Product added successfully",
    duration: 3000,
    /* destination: "https://github.com/apvarun/toastify-js", */
    newWindow: true,
    close: true,
    gravity: "top", // `top` or `bottom`
    position: "left", // `left`, `center` or `right`
    stopOnFocus: true, // Prevents dismissing of toast on hover
    style: {
      background: "linear-gradient(to right, #00b09b, #96c93d)",
    },
    onClick: function(){} // Callback after click
  }).showToast();
    console.log(products);
  }

  const updateProduct = () => {
    let updatedProduct = {
      name: product.name,
      description: product.description,
      id: product.id,
      imageURL: product.imageURL,
      category: product.category,
    }
    // compara el id del arreglo de products con los del arreglo "producto" que son los datos de entrada
    const productIndex = products.findIndex(p => p.id === product.id)
    products[productIndex] = updatedProduct;
    cleanProduct();
    editStatus = false;
    Toastify({
    text: "Product updated successfully",
    duration: 3000,
    /* destination: "https://github.com/apvarun/toastify-js", */
    newWindow: true,
    close: true,
    gravity: "top", // `top` or `bottom`
    position: "left", // `left`, `center` or `right`
    stopOnFocus: true, // Prevents dismissing of toast on hover
    style: {
      background: "linear-gradient(to right, #00b09b, #96c93d)",
    },
    onClick: function(){} // Callback after click
  }).showToast();
    editStatus = false;
  }


  const onSubmitHandler = (e) => {
    if (!editStatus){
      addProduct();
    }else{
      updateProduct();
      console.log('update product ');
    }
  };

  const deleteProduct = id => {
    console.log(id);
    products = products.filter(product => product.id !== id);
  };

  const editProduct = productEdited => {
    product = productEdited;
    editStatus = true; 
  }


</script>

<main>
  <div class="container p-4">
    <div class="row">
      <div class="col-md-6">

      {#each products as product}
        <div class="card mt-2">
          <div class="row">
            <div class="col-md-4">
              {#if !product.imageURL}
              <img src="images/no-products-found.png" alt="" class="img-fluid p-2">
              {:else}
              <img src="{product.imageURL}" alt="" class="img-fluid p-2">
              {/if}
            </div>
            <div class="col-md 8">
              <div class="card-body">
                <h5><strong>{product.name}</strong></h5>
              <span><small>{product.category}</small></span> 
              <p class="card-text">{product.description}</p>
              <button class="btn btn-danger" on:click={deleteProduct(product.id)}>Delete</button>
              <button class="btn btn-secondary" on:click={editProduct(product)}>Edit</button>
              </div>
            </div>
          </div>
        </div>
        
      {/each}

      </div>
      <div class="col-md-6">
        <div class="card">
          <div class="card-body">
            <form on:submit|preventDefault={onSubmitHandler}>
              <div class="form-group">
                <input
                bind:value={product.name}
                type="text"
                placeholder="Product Name"
                class="form-control "
              />
              </div>
              <div class="form-group">
                <textarea
                bind:value={product.description}
                id="prod_description"
                rows="3"
                placeholder="Product Description"
                class="form-control"
              />
              </div>
              <div class="form-group">
                <input
                bind:value={product.imageURL}
                type="url"
                name=""
                id="prod_image_url"
                placeholder="https://faztweb.com/"
                class="form-control"
              />
              </div>
              <div class="form-group">
                <select id="category" bind:value={product.category}>
                  <option value="laptops">Laptops</option>
                  <option value="perifericos">Periféricos</option>
                  <option value="Multimedia">Televisores</option>
                  <option value="servers">Servers</option>
                </select>
              </div>
              <button class="btn btn-primary">
                {#if !editStatus}Save Product {:else} Update Product{/if}
              </button>
            </form>
          </div>
        </div>
      </div>
    </div>
</main>

<style>
</style>
