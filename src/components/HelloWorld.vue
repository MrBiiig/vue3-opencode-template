<script setup>
import { ref } from 'vue'

const count = ref(0)
</script>

<template>
  <div class="hello-world">
    <h1>Less 示例</h1>
    <p>当前计数: {{ count }}</p>
    <button class="counter" @click="count++">增加</button>
  </div>
</template>

<style lang="less">
// 变量
@primary-color: #409eff;
@text-color: #333;
@bg-color: #f5f7fa;

// 混入
.border-radius(@radius: 4px) {
  border-radius: @radius;
}

.hello-world {
  padding: 24px;
  background: @bg-color;
  color: @text-color;

  h1 {
    color: @primary-color;
    margin-bottom: 16px;
  }

  .counter {
    padding: 8px 16px;
    background: @primary-color;
    color: #fff;
    border: none;
    .border-radius(4px);
    cursor: pointer;
    transition: opacity 0.3s;

    &:hover {
      opacity: 0.8;
    }

    &:active {
      transform: scale(0.98);
    }
  }
}
</style>
