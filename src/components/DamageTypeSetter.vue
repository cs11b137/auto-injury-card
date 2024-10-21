<template>
  <div class="damage-type-container">
    <!-- 输入区域 -->
    <div class="input-section">
      <el-input
        v-model="inputValue"
        placeholder="请输入伤害类型名称"
        @keyup.enter="handleAdd"
        class="damage-input"
      />
      <el-button type="primary" @click="handleAdd">添加</el-button>
    </div>

    <!-- 简单列表 -->
    <ul class="damage-list">
      <li v-for="(type, index) in damageTypes" :key="index" class="damage-item">
        <span class="item-name">{{ type.name }}</span>
        <el-button type="danger" link @click="handleDelete(index)">
          删除
        </el-button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { ElMessage } from "element-plus";

// 响应式数据
const inputValue = ref("");
const damageTypes = ref([]);

// 添加伤害类型
const handleAdd = () => {
  if (!inputValue.value.trim()) {
    ElMessage.warning("请输入伤害类型名称");
    return;
  }

  if (damageTypes.value.some((item) => item.name === inputValue.value.trim())) {
    ElMessage.warning("该伤害类型已存在");
    return;
  }

  damageTypes.value.push({
    name: inputValue.value.trim(),
  });
  inputValue.value = "";
};

// 删除伤害类型
const handleDelete = (index) => {
  damageTypes.value.splice(index, 1);
};
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
  margin-bottom: 15px;
}

.damage-input {
  flex: 1;
}

.damage-list {
  list-style: none;
  padding: 0;
  margin: 0;
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
</style>
