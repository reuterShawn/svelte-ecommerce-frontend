<script>
    import { Link, navigate } from "svelte-routing";
    
    function getCookie(name) {
      const value = `; ${document.cookie}`;
      const parts = value.split(`; ${name}=`);
      if (parts.length === 2) return parts.pop().split(";").shift();
    }
    
    const email = getCookie("email");
    const fullname = getCookie("fullname");
    const userId = getCookie("userId");
    
    const handleLogout = () => {
      document.cookie = "email=; path=/; max-age=0";
      document.cookie = "fullname=; path=/; max-age=0";
      document.cookie = "userId=; path=/; max-age=0";
      navigate("/login");
    };
    </script>
    
    <nav>
        <div class="nav-container">
   
            <div class="nav-links">
                <Link to="/">Home</Link>
                <Link to="/about">About</Link>
                <Link to="/">Shop</Link>
                {#if email}
                    <span>Welcome, {fullname}</span>
                    <Link to="/cart/w"><span class="menu">Cart</span></Link>
                    <a href="#" on:click|preventDefault={handleLogout}>Logout</a>
                {:else}
                    <Link to="/login">Login</Link>
                    <Link to="/register">Register</Link>
                {/if}
            </div>
        </div>
      </nav>
    
    <style>
    nav {
      background-color: #D2D2D2;
      color: black;
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
    
    .nav-links {
      display: flex;
      gap: 1em;
    }
    
    .nav-links a,
    .nav-links span {
      color: black;
      text-decoration: none;
      font-size: 1em;
    }
    
    .nav-links span {
      font-size: 1em;
    }
    .menu {
      padding: 15px;
    }
    .menu:hover {
      background-color: #71b3bf;
    }
    </style>