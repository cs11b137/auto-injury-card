<template>
  <div class="damage-type-container">
    <!-- 输入区域 -->
    <div class="input-section">
      <div class="input-title">设置伤害的类型:</div>
      <el-input
        v-model="inputValue"
        placeholder="请输入伤害类型"
        @keyup.enter="handleAdd"
        class="damage-input" />
      <el-button type="default" @click="handleAdd">添加</el-button>
    </div>

    <!-- 简单列表 -->
    <ul class="damage-list">
      <li v-for="(type, index) in damageTypes" :key="index" class="damage-item">
        <span class="item-name">{{ type }}</span>
        <el-button type="danger" link @click="handleDelete(index)">
          删除
        </el-button>
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { ref, defineExpose } from "vue";

// 响应式数据
const inputValue = ref("") as any;
const damageTypes = ref([]) as any;

// 添加伤害类型
const handleAdd = () => {
  if (!inputValue.value.trim()) {
    return;
  }

  if (damageTypes.value.some((item: any) => item === inputValue.value.trim())) {
    return;
  }

  damageTypes.value.push(inputValue.value.trim());
  inputValue.value = "";
};

// 删除伤害类型
const handleDelete = (index: any) => {
  damageTypes.value.splice(index, 1);
};

// 暴露方法
defineExpose({
  damageTypes,
});
</script>

<style scoped>
.damage-type-container {
  padding: 20px;
  max-width: 800px;
  margin: 0 auto;
}

.input-section {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
  align-items: center;
}

.damage-input {
  flex: 1;
}

.damage-list {
  list-style: none;
  padding: 0;
  margin: 0;
  border-bottom: 1px solid #ebeef5;
}

.damage-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 8px 0;
}

.damage-item:not(:last-child) {
  border-bottom: 1px solid #ebeef5;
}

.item-name {
  font-size: 14px;
  color: #606266;
}

.input-title {
  font-size: 14px;
}
</style>
