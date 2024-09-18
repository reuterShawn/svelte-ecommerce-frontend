<!-- Home.svelte -->
<script>
    import { onMount } from "svelte";
    import { Link } from "svelte-routing";
    import Navbar from "./Navbar.svelte";
    
let products = [];
let error = null;

onMount(async () => {
  try {
    const response = await fetch(
      "http://localhost:1337/api/products?populate=*",
      {
        method: 'GET',
        headers: {
          'Content-Type': 'application/json',
        },
      }
    );

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    const rawData = await response.text(); // Get raw response text
    console.log("Raw response:", rawData);

    const data = JSON.parse(rawData);
    console.log("Parsed data:", data);

    if (!data || !data.data) {
      throw new Error("Unexpected data structure");
    }

    products = data.data.map((item) => ({
      id: item.id,
      name: item.attributes.productName,
      description: item.attributes.productDescription,
      price: parseFloat(item.attributes.productPrice),
      imageUrl: item.attributes?.productImage?.data?.[0]?.attributes?.url,
    }));

    console.log("Processed products:", products);
  } catch (error) {
    console.error("Error fetching products:", error);
    error = error.message;
  }
});
</script>


    
    <Navbar />
    
    <main>
          <h1>Main Store</h1>
          <div class="products">
            {#each products as product}
              <div class="product-card">
                <img
                  src={`http://localhost:1337${product.imageUrl}`}
                  alt={product.name}
                />
                <div class="details">
                  <h2>{product.name}</h2>
                  <p>{product.description}</p>
                  <p class="price">${product.price.toFixed(2)}</p>
                  <Link to={`/product/${product.id}`}>
                    <button>View Product</button>
                  </Link>
                </div>
              </div>
            {/each}
          </div>
        </main>
    
    <style>
    html,
    body {
      margin: 0;
      padding: 0;
      width: 100%;
      height: 100%;
      overflow-x: hidden;
    }
    
    body {
      font-family: Arial, sans-serif;
      display: flex;
      flex-direction: column;
    }
    
    nav {
      background-color: #71b3bf;
      color: white;
      padding: 1em 0;
      width: 100%;
      box-sizing: border-box;
    }
    
    .nav-container {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 0 2em;
      max-width: 1200px;
      margin: 0 auto;
    }
    
    .nav-brand a {
      color: white;
      text-decoration: none;
      font-size: 1.5em;
      font-weight: bold;
    }
    
    .nav-links {
      display: flex;
      gap: 1em;
    }
    
    .nav-links a {
      color: white;
      text-decoration: none;
      font-size: 1em;
    }
    
    .nav-links a:hover {
      text-decoration: underline;
    }
    
    main {
      padding: 2em;
      width: 100%;
      box-sizing: border-box;
      flex: 1;
    }
    
    h1 {
      color: #ff3e00;
      text-transform: uppercase;
      font-size: 2.5em;
      font-weight: 300;
      margin-bottom: 1em;
    }
    
    .products {
      display: flex;
      flex-wrap: wrap;
      gap: 2em;
      justify-content: center;
    }
    
    .product-card {
      background: #fff;
      border: 1px solid #ddd;
      border-radius: 10px;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
      overflow: hidden;
      flex: 1 1 calc(33.333% - 2em);
      max-width: calc(33.333% - 2em);
      transition: transform 0.2s;
      box-sizing: border-box;
    }
    
    .product-card:hover {
      transform: scale(1.05);
    }
    
    .product-card img {
      width: 100%;
      height: 200px;
      object-fit: cover;
    }
    
    .details {
      padding: 1em;
      text-align: left;
    }
    
    .details h2 {
      margin: 0 0 0.5em;
      font-size: 1.5em;
      color: #333;
    }
    
    .details p {
      margin: 0;
      color: #666;
    }
    
    .price {
      margin-top: 0.5em;
      font-size: 1.2em;
      font-weight: bold;
      color: #ff3e00;
      padding-top: 10px;
    }
    button {
      background-color: transparent;
      color: #71b3bf;
      border: 1px solid #71b3bf;
      padding: 0.75em 1.5em;
      border-radius: 5px;
      cursor: pointer;
      font-size: 1em;
      transition: background-color 0.3s;
      margin-left: 30%;
      margin-top: 25px;
    }
    
    button:hover {
      background-color: #71b3bf;
      color: white;
      border: none;
    }
    
    @media (max-width: 768px) {
      .product-card {
        flex: 1 1 calc(50% - 2em);
        max-width: calc(50% - 2em);
      }
    }
    
    @media (max-width: 480px) {
      .product-card {
        flex: 1 1 100%;
        max-width: 100%;
      }
    }
    </style>