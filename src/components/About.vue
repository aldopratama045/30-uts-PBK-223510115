<template>
  <div class="about-container">
    <div class="about-content">
      <div class="about-logo"></div>
      <div class="about-text">
        <h1 class="about-title">Tentang Kami</h1>
        <p class="about-description">Halo! Nama saya Aldo Pratama,  mahasiswa Universitas Islam Riau. Saya sangat senang dengan akademik yang dinamis dan berpengalaman di sini. Universitas Islam Riau menawarkan lingkungan belajar yang inklusif dan beragam, di mana saya berharap dapat mengembangkan potensi akademik dan pribadi saya.</p>
        <button class="about-button">Pelajari Lebih Lanjut</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {

}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;700&display=swap');

.about-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #fafafa; 
  padding: 20px;
}

.about-content {
  display: flex;
  align-items: center;
  background-color: rgba(0, 0, 0, 0.7);
  padding: 40px;
  border-radius: 15px;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.3);
  max-width: 800px;
  width: 100%;
}

.about-logo {
  background-image: url('src/assets/Logo_UIR.png');
  background-size: contain;
  background-repeat: no-repeat;
  background-position: center;
  width: 500px;
  height: 500px;
  margin-right: 40px;
}

.about-text {
  text-align: left;
}

.about-title {
  font-size: 36px;
  font-weight: 700;
  color: #fcfcfc;
  margin-bottom: 20px;
  font-family: 'Roboto', sans-serif;
}

.about-description {
  font-size: 18px;
  color: #fdfcfc;
  margin-bottom: 30px;
  font-family: 'Roboto', sans-serif;
}

.about-button {
  background-color: #ff6f61;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 25px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  font-family: 'Roboto', sans-serif;
}

.about-button:hover {
  background-color: #ff3b2f;
}
</style>
