<script lang="ts">
  let lists = $state([]);
  let newListTitle = $state('');

  function createList() {
    if (newListTitle.trim()) {
      lists = [...lists, { id: Date.now(), title: newListTitle, items: [] }];
      newListTitle = '';
    }
  }

  function deleteList(id: number) {
    lists = lists.filter(list => list.id !== id);
  }
</script>

<main class="container mx-auto px-4 py-8 max-w-4xl">
  <h1 class="text-4xl font-bold text-center text-white mb-8">My Lists</h1>
  
  <div class="bg-white/10 backdrop-blur-lg border border-white/20 rounded-lg shadow-lg p-6 mb-8">
    <div class="flex gap-4">
      <input
        type="text"
        bind:value={newListTitle}
        placeholder="Enter list title"
        class="flex-1 px-4 py-2 bg-white/20 border border-white/30 rounded-lg focus:ring-2 focus:ring-white/50 focus:border-transparent outline-none text-white placeholder-white/70"
        on:keydown={(e) => e.key === 'Enter' && createList()}
      />
      <button 
        on:click={createList}
        class="px-6 py-2 bg-white/20 text-white rounded-lg hover:bg-white/30 transition-colors duration-200 focus:ring-2 focus:ring-white/50 focus:ring-offset-2 focus:ring-offset-transparent"
      >
        Create List
      </button>
    </div>
  </div>

  <div class="grid gap-4">
    {#each lists as list (list.id)}
      <div class="bg-white/10 backdrop-blur-lg border border-white/20 rounded-lg shadow-lg p-6 flex items-center justify-between">
        <h2 class="text-xl font-semibold text-white">{list.title}</h2>
        <div class="flex gap-3">
          <button 
            class="px-4 py-2 bg-red-500/50 text-white rounded-lg hover:bg-red-600/50 transition-colors duration-200"
            on:click={() => deleteList(list.id)}
          >
            Delete
          </button>
          <a 
            href="/list/{list.id}" 
            class="px-4 py-2 bg-white/20 text-white rounded-lg hover:bg-white/30 transition-colors duration-200 no-underline"
          >
            View List
          </a>
        </div>
      </div>
    {/each}
  </div>
</main>