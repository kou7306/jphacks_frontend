<script lang="ts">
  import { onMount } from 'svelte';

  let users: any[] = [];
  let loading = true;
  /**
   * @type {null}
   */
  let error:any = null;

  onMount(async () => {
    try {
      const response = await fetch(import.meta.env.VITE_API_URL + '/users', {
        method: 'GET',
        mode: 'cors',
        headers: {
          'Content-Type': 'application/json', // 指定したい場合
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
  <h1 class="text-3xl font-bold underline">Users List</h1>
  <li><a href="/about">About</a></li>
  {#if loading}
    <p>Loading...</p>
  {:else if error}
    <p>Error: {error}</p>
  {:else if users.length === 0}
    <p>No users found.</p>
  {:else}
    <ul>
      {#each users as user}
        <li>{user.user_id}</li>
        <!-- 必要なプロパティを表示 -->
      {/each}
    </ul>
  {/if}
</main>
