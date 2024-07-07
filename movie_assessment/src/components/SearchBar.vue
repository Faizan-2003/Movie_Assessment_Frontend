<template>
  <div class="search-bar">
    <input v-model="searchPrompt" @keyup.enter="search" placeholder="Search for a movie...">
    <button class="btn btn-primary" @click="search">Search</button>
    <button class="btn btn-secondary" @click="clear">Clear</button>
  </div>
</template>

<script>

export default {
  data() {
    return {
      searchPrompt: ''
    }
  },
  methods: {
    search() {
      this.$emit('search', this.searchPrompt);
    },
    clear() {
      this.searchPrompt = '';
      this.$emit('clear');
    }
  }
}
</script>

<style scoped>
.search-bar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 10px;
}

.search-bar input {
  flex: 1;
  padding: 8px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 4px;
  margin-right: 10px;
}

.search-bar button {
  padding: 8px 16px;
  font-size: 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.search-bar button.btn-primary {
  background-color: #007bff;
  color: white;
}

.search-bar button.btn-secondary {
  background-color: #6c757d;
  color: white;
}
</style>