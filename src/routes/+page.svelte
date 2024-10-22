<script>
  import { onMount } from 'svelte';
  
  let users = [];
  let loading = true;
  let error = null;

  onMount(async () => {
    try {
      const response = await fetch(import.meta.env.VITE_API_URL + '/users', {
        method: 'GET',
        mode: 'cors',
        headers: {
          "Content-Type": "application/json", // 指定したい場合
        },
      }); // Express APIのURL
      console.log(response);
      if (!response.ok) {
        throw new Error('Network response was not ok');
      }
      users = await response.json();
      console.log(users);   
    } catch (err) {
      error = err.message;
    } finally {
      loading = false;
    }
  });
</script>

<main>
  <h1>Users List</h1>

  {#if loading}
    <p>Loading...</p>
  {:else if error}
    <p>Error: {error}</p>
  {:else if users.length === 0}
    <p>No users found.</p>
  {:else}
    <ul>
      {#each users as user}
        <li>{user.user_id}</li> <!-- 必要なプロパティを表示 -->
      {/each}
    </ul>
  {/if}
</main>
