<script>
  import { v4 } from "uuid";
  import Noty from "noty";

  import 'noty/lib/noty.css';
  import 'noty/lib/themes/sunset.css';
  let products = [
    {
      id: "1",
      name: "Lenovo Laptop",
      description: "MARCA Laptop",
      category: "laptop"
    },
    {
      id: "2",
      name: "Monitor Raxer",
      description: "Monitor R",
      category: "monitor"
    }
  ];

  let product = {
    id: "",
    name: "",
    description: "",
    category: "",
    imageURL: ""
  };

  let editStatus = false;

  const cleanProduct = () => {
    product = {
      id: "",
      name: "",
      description: "",
      category: "",
      imageURL: ""
    };
  };

  const addProduct = () => {
	  

    const newProduct = {
      id: v4(),
      name: product.name,
      description: product.description,
      category: product.category,
      imageURL: product.imageURL
    };

    products = products.concat(newProduct);
    cleanProduct();
  }

  const updateProduct = () => {
	  let updateProduct = {
		  name: product.name,
		  description: product.description,
		  id: product.id,
		  imageURL: product.imageURL,
		  category: product.category
	  };
	  const productIndex = products.findIndex(p => p.id === product.id);
	  products[productIndex] = updateProduct;
	  cleanProduct();
	  editStatus = false;
	  new Noty({
		  theme: 'sunset',
		  type: 'success',
		  timeout: 3000,
		  text:'Producto Actualizado'
	  }).show();
  }

  const onSubmitHandler = e => {
	//e.preventDefault();
	
	
	if(!editStatus){
		addProduct();
	}else{
		updateProduct();
	}
		
  };

  const deleteProduc = id => {
    products = products.filter(product => product.id !== id);
  };

  const editProduct = productEdited => {
    product = productEdited;
	editStatus = true;
	//console.log(editStatus);
  };
</script>

<style>

</style>

<main>
  <div class="container p-4">
    <div class="row">
      <div class="col-md-6">
        {#each products as product}
          <div class="card mt-2">
            <div class="row">
              <div class="col-md-4">
                {#if !product.imageURL}
                  <img
                    src="images/no-product-image.jpg"
                    alt=""
                    class="img-fluid p-3" />
                {:else}
                  <img src={product.imageURL} alt="" class="img-fluid p-3" />
                {/if}
              </div>
              <div class="col-md-8">
                <div class="card-body">
                  <h5>
                    <strong>{product.name}</strong>
                    <span>
                      <small>{product.category}</small>
                    </span>
                  </h5>
                  <p class="card-text">{product.description}</p>
                  <button
                    class="btn btn-danger"
                    on:click={deleteProduc(product.id)}>
                    Eliminar
                  </button>
                  <button
                    class="btn btn-secondary"
                    on:click={editProduct(product)}>
                    Editar
                  </button>
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
                  class="form-control"
                  bind:value={product.name}
                  type="text"
                  placeholder="Nombre de Producto"
                  id="product-name" />
              </div>
              <div class="form-group">
                <textarea
                  class="form-control"
                  bind:value={product.description}
                  id="product-description"
                  rows="3" />
              </div>
              <div class="form-group">
                <input
                  class="form-control"
                  type="url"
                  bind:value={product.imageURL}
                  id="product-image-url"
                  placeholder="www.google.png" />
              </div>
              <div class="form-group">
                <select
                  class="form-control"
                  bind:value={product.category}
                  id="category">
                  <option value="laptops">Laptops</option>
                  <option value="peripherials">Peripherials</option>
                  <option value="servers">Servers</option>
                </select>
              </div>
              {#if !editStatus}
                <button class="btn btn-primary">Guardar</button>
              {:else}
                <button class="btn btn-primary">Actualizar</button>
              {/if}
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</main>
