<!-- App.svelte -->
<script>
	import { Router, Link, Route } from "svelte-routing";
	import Home from "./Components/Home.svelte";
	import Login from "./Components/Login.svelte";
	import Register from "./Components/Register.svelte";
	import Product from "./Components/Product.svelte";
	import Cart from "./Components/Cart.svelte";
	</script>
	
	<main>
		<Router>
		  <Route path="/" component="{Home}" />
		  <Route path="login" component="{Login}" />
		  <Route path="register" component="{Register}" />
		  <Route path="product/:id" component="{Product}" />
		  <Route path="cart/:id" component="{Cart}" />
		</Router>
	  </main>
	
	<style>
	main {
	  text-align: center;
	  padding: 1em;
	  max-width: 100%;
	  margin: 0 auto;
	}
	</style>

	