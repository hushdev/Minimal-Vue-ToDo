<template>
  <nav>
    <span class="fs-9 fw-b text-main">Мои задачи</span>
    <span class="fs-3 text-secondary">
      Test project by
      <a
        href="https://github.com/hushdev"
        class="text-primary"
        target="_blank"
      >hushdev</a>
    </span>
  </nav>
</template>


<style lang="scss" scoped>
nav {
  padding: 20px 0;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

@media (max-width: 480px) {
  nav {
    flex-direction: column;
    align-items: flex-start;
  }
}
</style>