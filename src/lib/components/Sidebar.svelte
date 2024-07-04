<script lang="ts">
  import { page } from '$app/stores';
  import { Briefcase, CheckSquare, Calendar, BarChart2, ChevronLeft, ChevronRight } from 'lucide-svelte';
  import '../styles/app.css'

  let isMinimized = false;

  const toggleSidebar = () => {
    isMinimized = !isMinimized;
  };

  $: currentPage = $page.url.pathname.split('/')[1] || 'Home';

  const menuItems = [
    { icon: Briefcase, label: 'Projects', href: '/projects' },
    { icon: CheckSquare, label: 'Tasks', href: '/tasks' },
    { icon: Calendar, label: 'Calendar', href: '/calendar' },
    { icon: BarChart2, label: 'Reports', href: '/reports' },
  ];
</script>

<aside class="bg-sky-950 text-white h-screen flex flex-col transition-all duration-300 ease-in-out {isMinimized ? 'w-14' : 'w-48'} shadow-md z-20">
  <div class="p-3 flex items-center justify-between h-14 bg-sky-200">
    {#if !isMinimized}
      <h2 class="text-lg font-semibold ">
        <a href="/"><img src="vc2-Photoroom.png" alt="logo" class="h-12" /></a>
      </h2>
    {/if}

    {#if isMinimized}
      <h2 class="text-lg font-semibold ">
        <a href="/"><img src="favicon.png" alt="logo" class="h-8" /></a>
      </h2>
    {/if}
    

  </div>
  <nav class="flex-1 overflow-y-auto">
    <ul class="space-y-1 p-2">
      {#each menuItems as item}
        <li>
          
           <a href={item.href}
            class="flex items-center p-2 rounded-md hover:bg-sky-800 transition-colors duration-200 {$page.url.pathname === item.href ? 'bg-sky-900' : ''}"
          >
            <svelte:component this={item.icon} size={20} />
            {#if !isMinimized}
              <span class="ml-3 text-sm">{item.label}</span>
            {/if}
          </a>
        </li>
      {/each}
    </ul>
  </nav>
  <button 
    on:click={toggleSidebar} 
    class="p-2 bg-sky-950 hover:bg-sky-800 transition-colors duration-200 flex justify-center items-center"
    aria-label={isMinimized ? "Expand sidebar" : "Collapse sidebar"}
  >
    <svelte:component this={isMinimized ? ChevronRight : ChevronLeft} size={20} />
  </button>
</aside>