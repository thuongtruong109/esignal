<template>
  <header class="flex items-center justify-between px-4 py-2">
    <div class="flex items-center space-x-1.5">
      <img src="/logo.png" alt="logo" width="auto" height="auto" class="w-8">
      <h1 class="text-2xl font-medium pr-2 pb-1.5 translate-y-1">
        Esignal
      </h1>
    </div>
    <UTooltip text="GitHub Repository">
      <NuxtLink to="https://github.com/thuongtruong109/esignal" target="_blank" class="flex items-center gap-2">
        <UIcon name="lucide:github" class="size-5 cursor-pointer" />
      </NuxtLink>
    </UTooltip>
  </header>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Pacifico&display=swap');
h1 {
  font-family: "Pacifico", serif;
  background: linear-gradient(90deg, #34d399, #3b82f6, #22d3ee);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
</style>
