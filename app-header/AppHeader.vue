<script lang="ts" setup>
const authStore = useAuthStore()
const isMounted = ref(false)
onMounted(async () => {
//   window.addEventListener('orientationchange', onOrientationChange)
  isMounted.value = true
})
</script>

<template>
  <div class="container p-x-16px">
    <div class="flex items-center justify-start items-stretch w-100px sm:w-auto">
      <mobile-button />
      <logotype />
      <navigation />
    </div>
    <div class="flex items-center justify-center sm:justify-end flex-1">
      <template v-if="isMounted">
        <span v-if="!authStore.isAuthenticated" class="md:pl-6px pl-0 md:mr-0 mr-16px">
          <login-button v-if="!authStore.isAuthenticated" />
        </span>
        <span v-if="authStore.isAuthenticated" class="pl-6px">
          <wallet-button v-if="authStore.isAuthenticated" />
        </span>
      </template>
      <template v-else>
        <span class="md:pl-6px pl-0 md:mr-0 mr-30px">
          <Skeleton height="48px" width="96px" />
        </span>
      </template>
    </div>
    <div class="flex items-center justify-end sm:justify-end w-100px sm:w-auto">
      <template v-if="isMounted">
        <span v-if="!authStore.isAuthenticated" class="pl-6px">
          <signup-button />
        </span>
        <span v-if="authStore.isAuthenticated" class="pl-6px">
          <deposit-button />
        </span>
        <span v-if="authStore.isAuthenticated" class="pl-6px">
          <profile-button />
        </span>
      </template>
      <template v-else>
        <span class="pl-6px">
          <Skeleton height="48px" width="96px" />
        </span>
      </template>
    </div>
  </div>
</template>

<style scoped>
.container {
    @apply h-60px sm:h-65px mb-17px overflow-hidden flex justify-start items-center items-stretch;
  }
</style>
