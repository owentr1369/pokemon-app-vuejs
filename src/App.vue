<template>
  <div id="app">
    <!-- <nav>
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </nav> -->
    <router-view />
  </div>
</template>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@100;200;300;400;500;600;700;800;900&display=swap");
body {
  display: flex;
  justify-content: center;
  align-items: center;
  width: calc(100% - 80px);
  flex-direction: column;
  padding: 10px;
  min-height: calc((100vh - 20px));
  background: linear-gradient(#156f99, #0a2e50);

  font-family: "Poppins", sans-serif;
  font-size: 1rem;
  font-weight: normal;
}
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
h1 {
  color: #efefef;
}
</style>
