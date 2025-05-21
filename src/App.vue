<template>
  <div class="container">
    <div v-if="!user" class="login-container">
      <h2>Login</h2>
      <form @submit.prevent="handleLogin">
        <div class="form-group">
          <input
            v-model="email"
            type="email"
            placeholder="Email"
            required
          />
        </div>
        <div class="form-group">
          <input
            v-model="password"
            type="password"
            placeholder="Password"
            required
          />
        </div>
        <button type="submit">Login</button>
      </form>
    </div>
    <div v-else class="content">
      <div class="header">
        <h2>Welcome, {{ user.email }}</h2>
        <button @click="handleLogout">Logout</button>
      </div>
      <div class="documents">
        <h3>Documents</h3>
        <div v-if="loading">Loading...</div>
        <div v-else-if="error">{{ error }}</div>
        <ul v-else>
          <li v-for="doc in documents" :key="doc.id">
            {{ doc.data().title || 'Untitled' }}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { 
  signInWithEmailAndPassword,
  signOut,
  onAuthStateChanged
} from 'firebase/auth'
import { 
  collection,
  getDocs
} from 'firebase/firestore'
import { auth, db } from './firebase'

const user = ref(null)
const email = ref('')
const password = ref('')
const documents = ref([])
const loading = ref(false)
const error = ref(null)

onMounted(() => {
  onAuthStateChanged(auth, (currentUser) => {
    user.value = currentUser
    if (currentUser) {
      fetchDocuments()
    }
  })
})

const handleLogin = async () => {
  try {
    await signInWithEmailAndPassword(auth, email.value, password.value)
    email.value = ''
    password.value = ''
  } catch (err) {
    error.value = err.message
  }
}

const handleLogout = async () => {
  try {
    await signOut(auth)
    documents.value = []
  } catch (err) {
    error.value = err.message
  }
}

const fetchDocuments = async () => {
  loading.value = true
  error.value = null
  try {
    const querySnapshot = await getDocs(collection(db, 'documents'))
    documents.value = querySnapshot.docs
  } catch (err) {
    error.value = err.message
  } finally {
    loading.value = false
  }
}
</script>

<style>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

.login-container {
  max-width: 400px;
  margin: 40px auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 8px;
}

.form-group {
  margin-bottom: 15px;
}

input {
  width: 100%;
  padding: 8px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  background-color: #4CAF50;
  color: white;
  padding: 10px 15px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #45a049;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.documents {
  background-color: #f9f9f9;
  padding: 20px;
  border-radius: 8px;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  padding: 10px;
  border-bottom: 1px solid #eee;
}

li:last-child {
  border-bottom: none;
}
</style> 