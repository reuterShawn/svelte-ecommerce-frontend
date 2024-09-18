<!-- Product.svelte -->
<script>
    import { onMount } from "svelte";
    import { Link, navigate } from "svelte-routing";
    import Navbar from "./Navbar.svelte";
    export let id;
    
    let product = null;
    let userId = getCookie("userId");
    let successMessage = "";
    
    function getCookie(name) {
      const value = `; ${document.cookie}`;
      const parts = value.split(`; ${name}=`);
      if (parts.length === 2) return parts.pop().split(";").shift();
    }
    
    onMount(async () => {
      if (!userId) {
        navigate("/login");
      } else {
        try {
          const response = await fetch(
            `http://localhost:1337/api/products/${id}?populate=*`,
          );
          const data = await response.json();
          const attributes = data.data.attributes;
          const imageUrl = attributes.productImage.data[0].attributes.url;
          product = {
            id: data.data.id,
            name: attributes.productName,
            description: attributes.productDescription,
            price: parseFloat(attributes.productPrice),
            imageUrl: `http://localhost:1337${imageUrl}`,
          };
        } catch (error) {
          console.error("Error fetching product:", error);
        }
      }
    });
    
    const addToCart = async () => {
      try {
        const response = await fetch(`http://localhost:1337/api/cart-items`, {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify({
            data: {
              userID: userId,
              productID: id,
            },
          }),
        });
    
        if (!response.ok) {
          throw new Error(response);
        }
    
        successMessage = "Product added to cart successfully!";
      } catch (error) {
        console.error("Error adding product to cart:", error);
      }
    };

    
    </script>
    
    <Navbar />
    <main>
                {#if product}
                    <div class="product-container">
                        <img src={product.imageUrl} alt={product.name} />
                        <div class="product-details">
                            <h1>{product.name}</h1>
                            <p>{product.description}</p>
                            <p class="price">${product.price.toFixed(2)}</p>
                            <button on:click={addToCart}>Add to Cart</button>
                            {#if successMessage}
                                <p class="success-message">{successMessage}</p>
                                <p><Link to="/cart/w">View Cart</Link></p>
                            {/if}
                            <p><Link to="/">Back to Home</Link></p>
                        </div>
                    </div>
                {:else}
                    <p>Product not found</p>
                {/if}
            </main>
    
    <style>
    html,
    body {
      margin: 0;
      padding: 0;
      width: 100%;
      height: 100%;
      overflow-x: hidden;
      font-family: Arial, sans-serif;
    }
    
    body {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
    }
    
    main {
      display: flex;
      justify-content: center;
      align-items: center;
      width: 100%;
      height: 100%;
      box-sizing: border-box;
      padding: 2em;
      background-color: #f9f9f9;
    }
    
    .product-container {
      display: flex;
      flex-direction: column;
      align-items: center;
      background: #fff;
      border: 1px solid #ddd;
      border-radius: 10px;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
      overflow: hidden;
      max-width: 600px;
      width: 100%;
      box-sizing: border-box;
      padding: 2em;
    }
    
    .product-container img {
      width: 50%;
      height: auto;
      border-bottom: 1px solid #ddd;
      margin-bottom: 1em;
    }
    
    .product-details {
      text-align: center;
    }
    
    h1 {
      color: #ff3e00;
      text-transform: uppercase;
      font-size: 2em;
      font-weight: 300;
      margin: 0 0 1em;
    }
    
    p {
      font-size: 1em;
      color: #666;
      margin: 0 0 1em;
    }
    
    .price {
      font-size: 1.5em;
      font-weight: bold;
      color: #ff3e00;
      margin: 0.5em 0;
    }
    
    button {
      background-color: #ff3e00;
      color: white;
      border: none;
      padding: 0.75em 1.5em;
      border-radius: 5px;
      cursor: pointer;
      font-size: 1em;
      transition: background-color 0.3s;
    }
    
    button:hover {
      background-color: #e03500;
    }
    
    .success-message {
      color: green;
      font-weight: bold;
      margin-top: 1em;
    }
    
    a {
      color: #ff3e00;
      text-decoration: none;
      font-size: 1em;
    }
    
    a:hover {
      text-decoration: underline;
    }
    </style>