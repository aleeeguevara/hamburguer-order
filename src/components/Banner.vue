<template>
  <div id="main-banner">
    <h1>Make Your Burger</h1>
  </div>
</template>

<script>

export default {
  name: 'BannerComponent',
};
</script>

<style scoped>
  #main-banner {
    background-image: url('/public/img/burger.jpg');
    background-position: 0 -250px;
    background-size: cover;
    height: 400px;
    display: flex;
    align-items: center;
    justify-content: flex-start;
    width: 100%;
    background-repeat: no-repeat;
  }

  #main-banner h1 {
    color: #fff;
    text-align: center;
    font-size: 60px;
    background-color: #222;
    padding: 20px 40px;

  }
</style>
