<script>
	import { v4 } from 'uuid'
	import Noty from 'noty'
	import 'noty/lib/noty.css'
	import 'noty/lib/themes/sunset.css'

	let products = [
		{
			id:1,
			name:'HP Pavilion',
			description:'New Laptop',
			category:'laptops'
		},
		{
			id:2,
			name:'Mouse Razer',
			description:'Mouse Gaming',
			category:'peripherials'
		}
	]
	let product = {id:'', name:'', description:'',category:'',imageURL:''}
	
	let editStatus = false
	const cleanProduct = () =>{
		product ={
			id:'',
			name:'',
			description:'',
			category:'',
			imageURL:''
		}
	}

	const addProduct = () =>{
		
		const newProduct = {
			id:v4(),
			name:product.name,
			description:product.description,
			category:product.category,
			imageURL:product.imageURL
		}

		products = products.concat(newProduct)
		cleanProduct()
	}

	const updateProduct = () =>{
		let updateProduct = {
			name:product.name,
			description:product.description,
			category:product.category,
			imageURL:product.imageURL,
			id:product.id
		}
		const productIndex = products.findIndex(p => p.id === product.id)
		products[productIndex] = updateProduct
		cleanProduct()
		editStatus = false
		new Noty({
			theme:'sunset',
			type:'success',
			timeout: 3000,
			text:'Product Update Successfully'
		}).show()
		
	}

	const onSubmitHandler = e =>{
		if(!editStatus){
			addProduct()
		}else{
			updateProduct()
		}
		
	}

	const deleteProduct = (id) =>{
		products = products.filter(product => product.id !== id)
	}

	const editProduct = productEdited => {
		product = productEdited
		editStatus = true
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
									<img class="img-fluid p-2" src="images/no-products-found.jpg" alt="">
								{:else}
								<img class="img-fluid p-2" src="{product.imageURL}" alt="">
								{/if}
							</div>
							<div class="col-md-8">
								<div class="card-body">
									<h5>
										<strong>
											{product.name}
										</strong>
										<span>
											<small>
												{product.category}
											</small>
										</span>
									</h5>
									<p class="card-text">{product.description}</p>
									<button on:click={deleteProduct(product.id)} class="btn btn-danger">Delete</button>
									<button on:click={editProduct(product)} class="btn btn-secondary">Edit</button>
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
								<input bind:value={product.name} type="text" placeholder="Product Name" id="product-name" class="form-control">	
							</div>
							<div class="form-group">
								<textarea bind:value={product.description} id="product-description" rows="3" placeholder="Product Description" class="form-control"></textarea>
							</div>
							<div class="form-group">
								<input bind:value={product.imageURL} type="url" id="product-img-url" placeholder="http://imagen.com" class="form-control">
							</div>
							<div class="form-group">
								<select id="category" bind:value={product.category} class="form-control">
									<option value="laptops">Laptops</option>
									<option value="peripherials">Peripherials</option>
									<option value="servers">Servers</option>
								</select>
							</div>
							<button class="btn btn-primary">
								{#if !editStatus}
									Save Product
								{:else}
									Update Product
								{/if}
							</button>
						</form>
					</div>
				</div>
			</div>
		</div>
	</div>
</main>

<style>
</style>