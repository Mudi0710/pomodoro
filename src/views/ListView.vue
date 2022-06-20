<template lang="pug">
v-row#list
  v-col(cols='12')
    h1 待辦事項
  v-col(cols='12')
    //-
      variant="underlined"    => 設定輸入欄位樣式（看文件）
      v-model="newItem"       => 綁定 newItem
      append-icon="mdi-plus"  => 設定 icon 樣式
      :rules="[required]"     => 套用 required 判斷規則
      @click:append="onInputSubmit" => icon 滑鼠點擊事件
      @keydown.enter='onInputSubmit'=> 鍵盤鍵入事件
  v-col(cols='12')
    v-text-field(
      variant="underlined"
      v-model="newItem"
      append-icon="mdi-plus"
      :rules="[required]"
      @click:append="onInputSubmit"
      @keydown.enter='onInputSubmit'
    )
    v-table
      thead
        tr
          th 名稱
          th 操作
      tbody
        tr(v-if="items.length === 0")
          td.text-center(colspan="2") 沒有資料
        tr(v-for="(item, i) in items")
          td
            v-text-field(v-if="item.edit" v-model="item.model" autofocus)
            span(v-else) {{ item.name }}
          td
            span(v-if="item.edit")
              v-btn(icon variant="plain" flat color="green" @click="confirmEditItem(i)")
                v-icon mdi-check
              v-btn(icon variant="plain" flat color="red" @click="cancelEditItem(i)")
                v-icon mdi-undo
            span(v-else)
              v-btn(icon variant="plain" flat color="green" @click="editItem(i)")
                v-icon mdi-pencil
              v-btn(icon variant="plain" flat color="red" @click="delItem(i)")
                v-icon mdi-delete
</template>

<script setup>
import { ref } from 'vue'
import { storeToRefs } from 'pinia'
import { useListStore } from '@/stores/list'

const newItem = ref('')
// 用 ! 把 value 變成布林值 (兩個 !! 代表負負得正)，用短路求值去判斷
const required = value => {
  return !!value || '必填'
}

const list = useListStore()
const { items } = storeToRefs(list)
const { addItem, delItem, editItem, confirmEditItem, cancelEditItem } = list

const onInputSubmit = () => {
  // 把 required 設成變數（為了比較好寫）
  const valid = required(newItem.value)
  // 判斷 newItem.value 是否為字串，如果是就回傳
  if (typeof valid === 'string') return
  addItem(newItem.value)
  // 然後再把 newItem 值清空
  newItem.value = ''
}
</script>
