<template>
  <div class="default">
    <nuxt />

    <UiFooter class="default__footer" />

    <ClientOnly>
      <TheGdpr v-if="shouldOpenGdpr" @cancel="closeGdpr" />
    </ClientOnly>
  </div>
</template>

<script>
import { onBeforeMount, onBeforeUnmount } from 'nuxt-composition-api'

import { setViewport } from '~/store/composition/viewport.js'
import { rAFWithDebounce } from '~/utils/index.js'

if (process.browser) {
  // eslint-disable-next-line no-var
  var {
    state: { shouldOpenGdpr },
    closeGdpr,
  } = require('~/composition/store/local-storage.js')
}

export default {
  setup() {
    useUpdateViewport()

    return {
      shouldOpenGdpr,
      closeGdpr,
    }
  },
}

function useUpdateViewport() {
  onBeforeMount(() => {
    updateViewport()

    window.addEventListener('resize', updateViewport)
    window.addEventListener('orientationChange', updateViewport)
  })
  onBeforeUnmount(() => {
    window.removeEventListener('resize', updateViewport)
    window.removeEventListener('orientationChange', updateViewport)
  })

  function updateViewport() {
    rAFWithDebounce(() => {
      const { clientWidth, clientHeight } = document.documentElement
      setViewport({
        width: clientWidth,
        height: clientHeight,
      })
    })
  }
}
</script>

<style lang="scss">
html {
  font-family: 'Futura PT', 'Noto Sans CJK TC', sans-serif;
}
.default {
  &__footer {
    max-width: 1096px;
    margin-left: auto;
    margin-right: auto;
    @include media-breakpoint-up(md) {
      padding-left: 20px;
      padding-right: 20px;
    }
    // 1136 = 1096 + 20 * 2
    @media (min-width: 1136px) {
      padding-left: 0;
      padding-right: 0;
    }
  }
}
</style>
