<template>
  <div class="input-list-container">
    <div class="input-container">
      <el-input
        v-model="inputValue"
        placeholder="请输入文字"
        @keyup.enter="addItem"
      >
        <template #append>
          <el-button @click="addItem">新增</el-button>
        </template>
      </el-input>
    </div>
    <div class="items-container">
      <el-tag
        v-for="(item, index) in items"
        :key="index"
        class="mx-1"
        closable
        @close="removeItem(index)"
      >
        {{ item }}
      </el-tag>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";

const inputValue = ref("") as any;
const items = ref([]) as any;

const addItem = () => {
  if (inputValue.value.trim()) {
    items.value.push(inputValue.value.trim());
    inputValue.value = "";
  }
};

const removeItem = (index: any) => {
  items.value.splice(index, 1);
};
</script>

<style scoped>
.input-list-container {
  width: 100%;
  max-width: 500px;
  margin: 0 auto;
}

.input-container {
  margin-bottom: 20px;
}

.items-container {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}
</style>
