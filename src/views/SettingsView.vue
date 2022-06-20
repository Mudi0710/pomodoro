<template lang="pug">
v-row#settings
  v-col(cols="12")
    h1.text-center 設定
  v-col(cols="12")
    v-table
      thead
        tr
          th 名稱
          th 試聽
          th 選擇
      tbody
        tr(v-for="alarm in alarms")
          td {{ alarm.name }}
          td
            audio(controls :src="alarm.file")
          td
            //- 不解構寫法
            //- v-radio(v-model="settings.selectedAlarm" :value="alarm.id")
            //- 解構寫法
            v-radio(v-model="selectedAlarm" :value="alarm.id")
</template>

<script setup>
import { useSettingsStore } from '@/stores/settings'
// 解構寫法 step 1
import { storeToRefs } from 'pinia'

const settings = useSettingsStore()
// 解構寫法 step 2
const { selectedAlarm, alarms } = storeToRefs(settings)
</script>
