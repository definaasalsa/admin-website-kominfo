<script lang="ts">
  import "../../app.css";
  import { goto } from "$app/navigation";
  import { onMount } from "svelte";
  import { User, Lock } from "lucide-svelte";

  let username = "";
  let password = "";
  let remember = false;
  let error = "";
  let loading = false;

  // akun dummy
  const dummyUser = {
    username: "admin",
    password: "12345"
  };

  async function handleLogin() {
    error = "";

    if (!username || !password) {
      error = "Semua field harus diisi!";
      return;
    }

    loading = true;

    setTimeout(() => {
      if (
        username === dummyUser.username &&
        password === dummyUser.password
      ) {
        goto("/beranda"); // redirect
      } else {
        error = "Username atau password salah!";
      }

      loading = false;
    }, 1000);
  }
</script>

<div class="min-h-screen flex items-center justify-center bg-gray-200">
  <div class="w-full max-w-md">

    <!-- Title -->
    <h1 class="text-2xl text-center font-semibold text-gray-700 mb-6">
      Dinas Kominfo Jatim
    </h1>

    <!-- card -->
    <div class="bg-white rounded-2xl shadow-md p-8">

      <p class="text-center text-gray-500 mb-6">
        Welcome back, please login to your account.
      </p>

      <!-- error -->
      {#if error}
        <div class="mb-4 text-sm text-red-500 bg-red-100 p-2 rounded">
          {error}
        </div>
      {/if}

      <!-- username -->
      <div class="relative mb-4">
        <User class="absolute left-3 top-3 text-gray-400" size={18} />
        <input
          type="text"
          placeholder="Nama pengguna"
          bind:value={username}
          class="w-full pl-10 pr-3 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-400"
        />
      </div>

      <!-- password -->
      <div class="relative mb-4">
        <Lock class="absolute left-3 top-3 text-gray-400" size={18} />
        <input
          type="password"
          placeholder="Kata sandi"
          bind:value={password}
          on:keydown={(e) => e.key === "Enter" && handleLogin()}
          class="w-full pl-10 pr-3 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-400"
        />
      </div>

      <!-- remember -->
      <div class="flex items-center mb-6">
        <input type="checkbox" bind:checked={remember} class="mr-2" />
        <span class="text-sm text-gray-600">Ingat saya</span>
      </div>

      <!-- button -->
      <div class="flex justify-end">
        <button
          on:click={handleLogin}
          disabled={loading}
          class="bg-indigo-500 hover:bg-indigo-600 text-white px-6 py-2 rounded-lg flex items-center gap-2 transition disabled:opacity-50"
        >
          {#if loading}
            Loading...
          {:else}
            Masuk →
          {/if}
        </button>
      </div>

      <!-- info login -->
      <p class="text-xs text-gray-400 mt-4 text-center">
        admin / 12345
      </p>

    </div>
  </div>
</div>