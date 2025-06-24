<script lang="ts" setup>
import ProviderList from '~/components/catalog/ProviderList.vue'
import TheBanner from '~/components/index/TheBanner.vue'
</script>

<template>
  <div class="content">
    <div class="p-b-24px">
      <TheBanner />
    </div>
    <div class="p-b-24px">
      <div class="md:flex md:justify-between block items-center gap-14px">
        <GameTagCarousel />
        <ProviderList />
      </div>
    </div>
    <NuxtPage />
  </div>
</template>

<style scoped>
.content {
  flex-grow: 1;
  flex-shrink: 2;
  padding: 0;
  max-width: 100%;
  overflow: hidden;
}
</style>
