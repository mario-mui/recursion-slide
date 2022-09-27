<script setup lang="ts">

defineProps<{
  step: number
  mapper?: Record<string, string[]>
}>()

</script>

<template>
  <div class="c">
    <div class="stack">
      <div class=fnc v-for="item in mapper[String(step)]" :key="item">
        {{ item }}
      </div>
    </div>
    <div class="title">
      step{{ step }}
    </div>
  </div>
</template>

<style scoped>
.c {
  position: absolute;
  bottom: -50px;
  right: 0;
}
.stack {
  height: 240px;
  width: 120px;
  border: 1px solid;
  border-top: none;
  display: flex;
  flex-direction: column-reverse;
  justify-content: end;
  align-items: center;
}
.title {
  margin-top: 10px;
  width: 120px;
  text-align: center;
}
.fnc {
  margin-bottom: 10px;
  background: #6ca0eab8;
  padding: 2px 10px;
  color: red;
}
</style>
