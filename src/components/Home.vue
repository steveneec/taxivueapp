<template>
  <div class="content">
    <div class="home-desc">
      <h1>Title Text</h1>
      <div id="logo" />
      <p>
        Lorem ipsum dolor sit amet consectetur, adipisicing elit. Excepturi
        laudantium repellat voluptas ipsam, nihil sunt aliquid eaque impedit
        architecto praesentium, quaerat perferendis suscipit. Assumenda esse
        ipsa cum tempora, saepe dolores.
      </p>
    </div>
  </div>
</template>

<style scoped>
#logo {
  height: 200px;
  width: 200px;
  border-radius: 50%;
  background-image: url("../assets/images/logo.png");
  background-size: 200%;
  background-position: center center;
}
p {
  color: #000;
}

.content {
  display: flex;
  justify-content: center;
}

.home-desc {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
  max-width: 600px;
}

.content h1 {
  font-weight: 600;
}
</style>
