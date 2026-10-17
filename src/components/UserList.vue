<script setup lang="ts">
import useSWRV from 'swrv'

interface User {
  id: number
  name: string
  email: string
}

async function fetchUsers(url: string): Promise<User[]> {
  const res = await fetch(url)
  if (!res.ok) throw new Error(`Request failed: ${res.status}`)
  return res.json()
}

const { data, error, isValidating, mutate } = useSWRV<User[]>(
  'https://jsonplaceholder.typicode.com/users',
  fetchUsers,
)
</script>

<template>
  <div class="user-list">
    <div class="header">
      <h2>使用者列表 (swrv)</h2>
      <button @click="() => mutate()" :disabled="isValidating">
        {{ isValidating ? '重新整理中…' : '重新整理' }}
      </button>
    </div>

    <p v-if="error" class="error">載入失敗：{{ error.message }}</p>
    <p v-else-if="!data" class="loading">載入中…</p>

    <ul v-else>
      <li v-for="user in data" :key="user.id">
        <strong>{{ user.name }}</strong> — {{ user.email }}
      </li>
    </ul>
  </div>
</template>

<style scoped>
.user-list {
  text-align: left;
  max-width: 480px;
  margin: 0 auto;
}

.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
}

.error {
  color: #e53e3e;
}

.loading {
  color: #666;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  padding: 0.5rem 0;
  border-bottom: 1px solid #eee;
}
</style>
