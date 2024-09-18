<!-- Login.svelte -->
<script>
  import { createEventDispatcher } from "svelte";
  import { navigate } from "svelte-routing";
  
  let email = "";
  let password = "";
  let userId = "";
  const dispatch = createEventDispatcher();
  
  const handleLogin = async () => {
    try {
      const response = await fetch(
        `http://localhost:1337/api/user-infos?filters[email][$eq]=${email}`,
      );
      if (!response.ok) {
        throw new Error("Error checking user login status");
      }
      const data = await response.json();
      if (data.data.length === 0) {
        alert("User not found");
        return;
      }
      const user = data.data[0].attributes;
      if (user.password !== password) {
        alert("Incorrect password");
        return;
      }
      alert(`Logged in successfully`);
      navigate(`/`);
      document.cookie = `email=${user.email}; path=/; max-age=${7 * 24 * 60 * 60}`;
      document.cookie = `fullname=${user.fullname}; path=/; max-age=${7 * 24 * 60 * 60}`;
      document.cookie = `userId=${data.data[0].id}; path=/; max-age=${7 * 24 * 60 * 60}`;
    } catch (error) {
      alert(`Error: ${error.message}`);
    }
  };
  
  const handleRegisterLinkClick = () => {
    navigate(`/register`);
  };
  </script>
  
  <main>
          <h1>Login</h1>
          <form on:submit|preventDefault={handleLogin}>
              <label>
                  Email:
                  <input type="email" bind:value={email} required>
              </label>
              <label>
                  Password:
                  <input type="password" bind:value={password} required>
              </label>
              <button type="submit">Login</button>
          </form>
          <p>Don't have an account? <a href="register" on:click={handleRegisterLinkClick}>Register</a></p>
      </main>
  
  <style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }
  
  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 2em;
    font-weight: 100;
  }
  
  label {
    display: block;
    margin: 0.5em 0;
  }
  
  input {
    width: 100%;
    padding: 0.5em;
    margin: 0.5em 0;
  }
  
  button {
    padding: 0.5em 1em;
    background-color: #ff3e00;
    color: white;
    border: none;
    cursor: pointer;
  }
  
  button:hover {
    background-color: #e63600;
  }
  
  p {
    margin-top: 1em;
  }
  
  a {
    color: #ff3e00;
    text-decoration: none;
  }
  
  a:hover {
    text-decoration: underline;
  }
  </style>