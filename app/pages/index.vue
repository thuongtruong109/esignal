<script setup lang="ts">
import type { SignatureFormData, SignatureOptions } from '~~/types'

const options = ref<SignatureOptions>({
  gap: {
    title: 10,
    subtitle: 10,
    social: 10,
    image: 10,
  },
  size: {
    title: 16,
    subtitle: 14,
    social: 14,
  },
  color: {
    title: '#000000',
    autoTitle: true,
    subtitle: '#848484',
    social: '#848484',
    background: '#ffffff',
    transparent: true,
  },
  image: {
    align: 'center',
    form: 'circle',
    size: 90,
  },
})

const data = ref<SignatureFormData>({
  image: '/avatar.jpg',
  fullName: 'Tran Nguyen Thuong Truong',
  jobTitle: 'Web Developer',
  company: '',
  email: '',
  phone: '(+84) 999 999 999',
  socials: [
  {
      title: 'Portfolio',
      url: '',
      type: 'portfolio',
    },
    {
      title: 'LinkedIn',
      url: 'https://www.linkedin.com/in/thuongtruong1009/',
      type: 'linkedin',
    },
    {
      title: 'Facebook',
      url: 'https://facebook.com/truong.poetic',
      type: 'facebook',
    },
    {
      title: 'Instagram',
      url: 'https://www.instagram.com/truong.poetic/',
      type: 'instagram',
    },
    {
      title: 'GitHub',
      url: 'https://github.com/thuongtruong1009',
      type: 'github',
    },
    {
      title: 'Twitter',
      url: '',
      type: 'twitter',
    },
  ],
})

</script>

<template>
  <div class="flex h-full flex-col relative space-y-10 p-5">
    <SettingsOption v-model:options="options" v-model:data="data" />
    <SignaturePreview
      :data
      :options
    />
  </div>
</template>
