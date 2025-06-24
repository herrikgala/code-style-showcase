<script setup lang="ts">
const websiteStore = useWebsiteStore()
const livechatStore = useLiveChatStore()
const chatButton = ref(null)
function positionTopUpButton() {
  const button = document.getElementById(`btn-top-up`)
  const main = document.getElementById(`main_content`)
  if (!main || !button)
    return

  let right: number
  if (window.innerWidth <= 425)
    right = 5
  else if (window.innerWidth <= 1640)
    right = main?.offsetLeft + 10
  else
    right = main?.offsetLeft - 10

  button.style.right = `${right}px`
  chatButton.value.style.right = `${right}px`
}

function handlerOnscroll() {
  const button = document.getElementById(`btn-top-up`)
  if (!button)
    return
  if (window.scrollY > 610)
    button.classList.add('top-up-enter')
  else
    button.classList.remove('top-up-enter')
}

function scrollToTop() {
  window.scrollTo({
    top: 0,
    behavior: 'smooth',
  })
}

async function waitForScrollHeight(targetHeight: number, timeout = 3000) {
  const interval = 50
  let waited = 0

  return new Promise<void>((resolve) => {
    const check = () => {
      if (document.body.scrollHeight >= targetHeight || waited >= timeout) {
        resolve()
      }
      else {
        waited += interval
        setTimeout(check, interval)
      }
    }
    check()
  })
}

async function scrollToSavedPosition() {
  const savedData = JSON.parse(localStorage.getItem('prevPageUrl'))
  if (savedData && savedData.path) {
    await nextTick()
    await waitForScrollHeight(savedData.scroll)

    window.scrollTo(0, savedData.scroll)

    setTimeout(() => {
      localStorage.removeItem('prevPageUrl')
    }, 100)
  }
}

// Init Anjouan seal
onMounted(() => {
  window.addEventListener('resize', positionTopUpButton)
  window.addEventListener('scroll', handlerOnscroll)
  positionTopUpButton()
  setTimeout(() => {
    try {
      if (anj_5d3544f8_661a_4878_aba1_a2ae8652323e && anj_5d3544f8_661a_4878_aba1_a2ae8652323e?.init) {
        //
        anj_5d3544f8_661a_4878_aba1_a2ae8652323e?.init()
      }
    }
    catch (e) {
      console.error(e)
    }
  }, 0)
  scrollToSavedPosition()
})

onBeforeUnmount(() => {
  window.removeEventListener('resize', positionTopUpButton)
  window.removeEventListener('scroll', handlerOnscroll)
})
const hover = ref(false)
</script>

<template>
  <div class="bg-bg">
    <header class="bg-surface-50 sm:bg-surface-100 sticky top-0 z-12">
      <CommonHeader />
    </header>
    <main id="main_content" class="min-h-[calc(100vh-100px)]">
      <slot />
    </main>
    <div
      v-if="websiteStore.value.showSupport"
      id="btn-chat"
      ref="chatButton"
      class="fixed bottom-6 z-10 transition-all duration-300"
      @click="livechatStore.openLiveChat"
    >
      <div class="relative">
        <CommonButton
          class="!h-12 !w-12 !rounded-full !p-0 shadow-lg"
          @mouseover="hover = true"
          @mouseleave="hover = false"
        >
          <CommonIcon name="chat" class="h-6 w-6 text-white" />
        </CommonButton>
        <div
          class="bg-surface-300 text-normal absolute bottom-1/2 right-full mr-3 w-max translate-y-1/2 rounded-lg px-3 py-2 text-sm transition-all duration-300"
          :class="{
            'visible opacity-100': hover,
            'invisible opacity-0': !hover,
          }"
        >
          {{ $t('liveChat') }}
          <div
            class="bg-surface-300 absolute bottom-1/2 right-[-3px] h-2 w-2 translate-y-1/2 rotate-45"
          />
        </div>
      </div>
    </div>
    <div
      id="btn-top-up"
      class="fixed bottom-20 z-10 transition-all duration-300"
      @click="scrollToTop"
    >
      <CommonButton
        class="!h-12 !w-12 !rounded-full !p-0 shadow-lg"
        :title="$t('toTop')"
      >
        <CommonIcon name="arrow-up" class="h-6 w-6 text-white" />
      </CommonButton>
    </div>
    <CommonFooter />
    <ModalsRootModal />
    <UtilsNotification />
    <UtilsTimer />
  </div>
</template>

<style>
#btn-top-up {
  opacity: 0;
  visibility: hidden;
}

#btn-top-up.top-up-enter {
  opacity: 1;
  visibility: visible;
}
</style>