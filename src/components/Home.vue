<template>
    <div class="home-container">
      <div class="home-content">
        <h1 class="home-title">WELCOME TO MY WEBSITE PRATAMA</h1>
        <p class="home-description">
            Temukan dengan melihat kegiatan Dengan Web ini. Mari Jelajahi dengan semua tentang yang ada di Universitas Islam Riau, Cerita Tentang Kegiatan, Foto Yang Mengagumkan. Saya sedang menempuh kuliah di Universitas Islam Riau.
        </p>
        <router-link to="/about" class="btn" @click="handleAboutClick">Tentang Kami</router-link>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    methods: {
      handleAboutClick() {
        this.$router.push('/about');
      }
    }
  }
  </script>
  
  <style>
 @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');
@import url('https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css');

.home-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background: 
    linear-gradient(
      to bottom right,
      rgba(255, 255, 255, 0.3) 0%,
      rgba(200, 200, 200, 0.6) 100%
    ),
    url('https://www.example.com/path-to-an-elegant-pattern.jpg');
  background-size: cover;
  font-family: 'Roboto', sans-serif;
  position: relative;
  overflow: hidden;
}

.home-content {
  text-align: center;
  background: rgba(255, 255, 255, 0.6);
  padding: 40px;
  border-radius: 15px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.3);
  animation: fadeIn 2s ease-in-out;
}

.home-title {
  font-size: 48px;
  font-weight: bold;
  color: #333;
  margin-bottom: 20px;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
  animation: slideInFromLeft 1s ease-in-out;
}

.home-description {
  font-size: 20px;
  color: #333;
  margin-bottom: 30px;
  animation: slideInFromRight 1s ease-in-out;
}

.btn {
  padding: 12px 25px;
  background: linear-gradient(135deg, #6a11cb, #2575fc);
  color: white;
  text-decoration: none;
  border-radius: 30px;
  font-weight: bold;
  box-shadow: 0 4px 15px rgba(106, 17, 203, 0.3);
  transition: all 0.3s ease;
}

.btn:hover {
  background: linear-gradient(135deg, #3b76e1, #0b57a3);
  box-shadow: 0 6px 20px rgba(106, 17, 203, 0.5);
  transform: translateY(-2px);
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes slideInFromLeft {
  from { transform: translateX(-100%); opacity: 0; }
  to { transform: translateX(0); opacity: 1; }
}

@keyframes slideInFromRight {
  from { transform: translateX(100%); opacity: 0; }
  to { transform: translateX(0); opacity: 1; }
}

  </style>
  