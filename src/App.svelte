<script>
  import { onMount } from "svelte";
  export let date;

  onMount(async () => {
    const res = await fetch("/api/date");
    const newDate = await res.text();
    date = newDate;
  });

import {
  Auth0Context,
  Auth0LoginButton,
  Auth0LogoutButton,
  authError,
  authToken,
  idToken,
  isAuthenticated,
  isLoading,
  login,
  logout,
  userInfo,
} from '@dopry/svelte-auth0';
</script>

<main>
  <h1>Svelte + Autho0 API</h1>
  <br />
  <h2>The date according to Node.js is:</h2>
  <p>{date ? date : 'Loading date...'}</p>
</main>

<Auth0Context domain="dev-hvw40i79.auth0.com" client_id="aOijZt2ug6Ovgzp0HXdF23B6zxwA6PaP">
  <Auth0LoginButton class="btn">Login</Auth0LoginButton>
  <Auth0LogoutButton class="btn">Logout</Auth0LogoutButton>
  <pre>isLoading: {$isLoading}</pre>
  <pre>isAuthenticated: {$isAuthenticated}</pre>
  <pre>authToken: {$authToken}</pre>
  <pre>idToken: {$idToken}</pre>
  <pre>userInfo: {JSON.stringify($userInfo, null, 2)}</pre>
  <pre>authError: {$authError}</pre>
</Auth0Context>
