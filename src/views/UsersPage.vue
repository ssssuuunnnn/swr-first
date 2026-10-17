<script setup lang="ts">
import useSWRV from 'swrv'

interface ApiUser {
  id: number
  created_at: string
}

const API_URL = 'https://ho4k23ylqydtgi5on32xdvzf5u0avsuv.lambda-url.ap-southeast-2.on.aws/users'

async function fetchUsers(url: string): Promise<ApiUser[]> {
  const res = await fetch(url)
  if (!res.ok) throw new Error(`Request failed: ${res.status}`)
  return res.json()
}

const { data, error, isValidating, mutate } = useSWRV<ApiUser[]>(API_URL, fetchUsers, {
  ttl: 2000,
  refreshInterval: 2000,
})

function formatDate(value: string): string {
  return new Date(value).toLocaleString()
}
</script>

<template>
  <div class="users-page">
    <div class="header">
      <h2>Users API</h2>
      <button @click="() => mutate()" :disabled="isValidating">
        {{ isValidating ? '重新整理中…' : '重新整理' }}
      </button>
    </div>

    <p v-if="error" class="error">載入失敗：{{ error.message }}</p>
    <p v-else-if="!data" class="loading">載入中…</p>

    <table v-else>
      <thead>
        <tr>
          <th>ID</th>
          <th>Created At</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="user in data" :key="user.id">
          <td>{{ user.id }}</td>
          <td>{{ formatDate(user.created_at) }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped>
.users-page {
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

table {
  width: 100%;
  border-collapse: collapse;
}

th,
td {
  text-align: left;
  padding: 0.5rem 0;
  border-bottom: 1px solid #eee;
}
</style>
