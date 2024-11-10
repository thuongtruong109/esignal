<script setup lang="ts">
import type { Signature } from '~~/types'
import * as htmlToImage from 'html-to-image';

const { data, options } = defineProps<Signature>()

const toast = useToast()

const copyButtonText = ref('Copy HTML')

const signatureContainer = ref<HTMLElement>()

const { copy, copied } = useClipboard({ source: () => signatureContainer.value.innerHTML, copiedDuring: 2000 })

function copyToClipboard() {
  try {
    copy()
    toast.add({
      title: 'Copied!',
      description: 'Signature copied to clipboard',
      icon: 'lucide:clipboard-check',
      variant: 'success',
      timeout: 2000,
    })
  } catch (error) {
    toast.add({
      title: 'Error',
      description: 'Failed to copy signature to clipboard',
      icon: 'lucide:alert-circle',
      variant: 'danger',
      timeout: 2000,
    })
    console.error(error)
  }
}

function exportAsImage() {
  htmlToImage.toPng(signatureContainer.value).then((dataUrl) => {
    const link = document.createElement('a')
    link.download = 'signature.png'
    link.href = dataUrl
    link.click()
  })
}
</script>

<template>
  <div class="flex w-full flex-col items-center justify-center space-y-4 px-4">
    <div ref="signatureContainer" class="w-fit rounded-md relative z-0">
      <img src="/sample_card_bg.png" alt="sample_bg" class="absolute inset-0 size-full rounded-md -z-10">
      <ClientOnly>
        <table :style="options.color.transparent ? {} : { backgroundColor: `${options.color.background}` }" class="w-full z-10 m-4">
          <tbody>
            <tr>
              <td style="padding: 6px;" :style="{ width: `${options.image.size + options.gap.image}px` }">
                <img
                  :src="data.image"
                  alt="Profile Picture"
                  :style="[
                    options.image.form === 'rectangle' ? { width: `${options.image.size}px` } : {},
                    options.image.form === 'square' ? { width: `${options.image.size}px`, height: `${options.image.size}px` } : {},
                    options.image.form === 'circle' ? { width: `${options.image.size}px`, height: `${options.image.size}px`, borderRadius: `${options.image.size}px` } : {},
                    { objectFit: 'cover' },
                  ]"
                >
              </td>
              <td style="padding: 6px; font-family: 'Inter', sans-serif;" :style="{ fontSize: `${options.size.subtitle}px` }">
                <table :style="{ fontSize: `${options.size.subtitle}px` }">
                  <tr v-if="data.fullName">
                    <td :style="{ fontWeight: '600', fontSize: `${options.size.title}px`, color: options.color.autoTitle ? '' : options.color.title }">
                      {{ data.fullName }}
                    </td>
                  </tr>
                  <tr v-if="data.jobTitle" :style=" { color: `${options.color.subtitle}`}">
                    <td>
                      {{ data.jobTitle }}
                    </td>
                  </tr>
                  <tr v-if="data.company" :style=" { color: `${options.color.subtitle}`}">
                    <td :style="{ fontSize: `${options.size.subtitle}px`, color: `${options.color.subtitle}` }">
                      {{ data.company }}
                    </td>
                  </tr>
                  <tr v-if="data.email" :style=" { color: `${options.color.subtitle}`}">
                    <td :style="{ fontSize: `${options.size.subtitle}px`, color: `${options.color.subtitle}` }">
                      {{ data.email }}
                    </td>
                  </tr>
                  <tr v-if="data.phone" :style=" { color: `${options.color.subtitle}`}">
                    <td>
                      {{ data.phone }}
                    </td>
                  </tr>
                  <tr v-if="data.socials.length > 0">
                    <td>
                      <table>
                        <tbody>
                          <tr :style="{ fontSize: `${options.size.social}px` }">
                            <td v-for="social in data.socials.filter((social) => social.url)" :key="social.title" :style="{ paddingRight: `${options.gap.social}px` }">
                              <a :href="social.url" style="text-decoration: underline" :style="{ color: `${options.color.social}` }">{{ social.title }}</a>
                            </td>
                          </tr>
                        </tbody>
                      </table>
                    </td>
                  </tr>
                </table>
              </td>
            </tr>
          </tbody>
        </table>
        <template #fallback>
          <div class="h-28" />
        </template>
      </ClientOnly>
    </div>
    <div class="flex space-x-4">
      <UButton
        :label="!copied ? copyButtonText : 'Copied!'"
        class="mt-4"
        color="primary"
        :disabled="copied"
        icon="lucide:copy"
        @click="copyToClipboard()"
      />

      <UButton
        label="Export image"
        class="mt-4"
        color="green"
        icon="lucide:image"
        @click="exportAsImage()"
      />
    </div>
  </div>
</template>

