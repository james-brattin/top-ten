<script lang="ts">
  import { page } from '$app/stores';
  
  let list = $state({ id: 0, title: '', items: [] });
  let newItem = $state('');

  function addItem() {
    if (newItem.trim()) {
      list.items = [...list.items, { id: Date.now(), text: newItem, completed: false }];
      newItem = '';
    }
  }

  function toggleItem(id: number) {
    list.items = list.items.map(item =>
      item.id === id ? { ...item, completed: !item.completed } : item
    );
  }

  function deleteItem(id: number) {
    list.items = list.items.filter(item => item.id !== id);
  }
</script>

<main class="container mx-auto px-4 py-8 max-w-4xl">
  <div class="mb-8">
    <a 
      href="/" 
      class="text-white/90 hover:text-white font-medium flex items-center gap-2 no-underline"
    >
      <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
        <path fill-rule="evenodd" d="M9.707 16.707a1 1 0 01-1.414 0l-6-6a1 1 0 010-1.414l6-6a1 1 0 011.414 1.414L5.414 9H17a1 1 0 110 2H5.414l4.293 4.293a1 1 0 010 1.414z" clip-rule="evenodd" />
      </svg>
      Back to Lists
    </a>
  </div>
  
  <div class="bg-white/10 backdrop-blur-lg border border-white/20 rounded-lg shadow-lg p-6 mb-8">
    <h1 class="text-3xl font-bold text-white mb-6">{list.title}</h1>

    <div class="flex gap-4 mb-6">
      <input
        type="text"
        bind:value={newItem}
        placeholder="Add new item"
        class="flex-1 px-4 py-2 bg-white/20 border border-white/30 rounded-lg focus:ring-2 focus:ring-white/50 focus:border-transparent outline-none text-white placeholder-white/70"
        on:keydown={(e) => e.key === 'Enter' && addItem()}
      />
      <button 
        on:click={addItem}
        class="px-6 py-2 bg-white/20 text-white rounded-lg hover:bg-white/30 transition-colors duration-200 focus:ring-2 focus:ring-white/50 focus:ring-offset-2 focus:ring-offset-transparent"
      >
        Add Item
      </button>
    </div>

    <ul class="space-y-3">
      {#each list.items as item (item.id)}
        <li class="flex items-center gap-4 p-4 bg-white/5 border border-white/10 rounded-lg group">
          <input
            type="checkbox"
            checked={item.completed}
            on:change={() => toggleItem(item.id)}
            class="w-5 h-5 bg-white/20 border-white/30 rounded focus:ring-white/50 checked:bg-white/40"
          />
          <span class="flex-1 text-white {item.completed ? 'line-through opacity-50' : ''}">{item.text}</span>
          <button 
            class="opacity-0 group-hover:opacity-100 px-3 py-1 bg-red-500/50 text-white rounded hover:bg-red-600/50 transition-all duration-200"
            on:click={() => deleteItem(item.id)}
          >
            Delete
          </button>
        </li>
      {/each}
    </ul>
  </div>
</main>