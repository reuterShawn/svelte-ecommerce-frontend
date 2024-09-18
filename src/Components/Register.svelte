<!-- Register.svelte -->
<script>
  import { createEventDispatcher } from "svelte";
  import { navigate } from "svelte-routing";
  let email = "";
  let fullname = "";
  let password = "";
  const dispatch = createEventDispatcher();
  
  const handleRegister = async () => {
    try {
      const checkResponse = await fetch(
        `http://localhost:1337/api/user-infos?filters[email][$eq]=${email}`,
      );
      if (!checkResponse.ok) {
        throw new Error("Error checking user registration status");
      }
      const existingUsers = await checkResponse.json();
  
      if (existingUsers.data.length > 0) {
        alert("User with this email is already registered");
        return;
      }
      const response = await fetch("http://localhost:1337/api/user-infos", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          data: {
            email: email,
            fullname: fullname,
            password: password,
          },
        }),
      });
  
      if (!response.ok) {
        const errorData = await response.json();
        throw new Error(errorData.error.message || "Registration failed");
      }
  
      alert(`Registered successfully.`);
      navigate(`/login`);
    } catch (error) {
      alert(`Error: ${error.message}`);
    }
  };
  
  const handleLoginLinkClick = () => {
    navigate(`/login`);
  };
  </script>
  
  <main>
          <h1>Register</h1>
          <form on:submit|preventDefault={handleRegister}>
              <label>
                  Fullname:
                  <input type="text" bind:value={fullname} required>
              </label>
              <label>
                  Email:
                  <input type="email" bind:value={email} required>
              </label>
              <label>
                  Password:
                  <input type="password" bind:value={password} required>
              </label>
              <button type="submit">Register</button>
          </form>
          <p>Already have an account? <a href="login" on:click|preventDefault={handleLoginLinkClick}>Login</a></p>
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