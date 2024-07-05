<script lang="ts">
  import { page } from '$app/stores';
  import { Briefcase, CheckSquare, Calendar, BarChart2, ChevronLeft, ChevronRight, LogOut, User } from 'lucide-svelte';
  import '../styles/app.css'
	import { applyAction, enhance } from '$app/forms';
	import { invalidateAll } from '$app/navigation';

  let isMinimized = false;

  const toggleSidebar = () => {
    isMinimized = !isMinimized;
  };

  $: currentPage = $page.url.pathname.split('/')[1] || 'Home';

  const menuItems = [
    { icon: Briefcase, label: 'Sales Orders', href: '/salesOrder' },
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
      {#if $page.data.user.role === 'ADMIN'}
      <li>
          
           <a href="/admin"
            class="flex items-center p-2 rounded-md hover:bg-sky-800 transition-colors duration-200 '/admin' ? 'bg-sky-900' : ''}"
          >
            <User size={20} />
            {#if !isMinimized}
              <span class="ml-3 text-sm">Admin</span>
            {/if}
          </a>
        </li>
        {/if}
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
      
  {#if $page.data.user}
      <form action="/logout" method="POST" use:enhance={() => {
        return async ({result}) => {
          invalidateAll()
          await applyAction(result)
        }
      }}>
        <button type="submit" class="flex items-center p-2 rounded-md hover:bg-sky-800 transition-colors duration-200">
          <LogOut size={20} />
            {#if !isMinimized}
              <span class="ml-3 text-sm">LogOut</span>
            {/if}
        </button>
      </form>
    {/if}

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