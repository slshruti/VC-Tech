<script lang="ts">
  import '../app.css';
  import Header from '$lib/components/Header.svelte';
  import Sidebar from '$lib/components/Sidebar.svelte';
  import Footer from '$lib/components/Footer.svelte';
</script>

<div class="flex h-screen bg-gray-50">
  <Sidebar />
  <div class="flex flex-col flex-1 overflow-hidden">
    <Header />
    <main class="flex-1 overflow-x-hidden overflow-y-auto bg-gray-50 p-4">
      <slot />
    </main>
    <Footer />
  </div>
</div>