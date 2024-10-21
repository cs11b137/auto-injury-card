<template>
  <div class="json-viewer-container">
    <el-input
      type="textarea"
      v-model="jsonInput"
      :rows="5"
      placeholder="请输入 JSON 数据"
      @input="updateJsonData"
    ></el-input>
    <el-button type="primary" @click="formatJson" :disabled="!isValidJson"
      >格式化 JSON</el-button
    >
    <el-button @click="clearJson">清除</el-button>
    <div v-if="errorMessage" class="error-message">{{ errorMessage }}</div>
    <div v-if="jsonData" class="json-view-wrapper">
      <vue-json-pretty
        :data="jsonData"
        :deep="1"
        :show-double-quotes="true"
        :show-length="true"
        @click="handleClick"
      ></vue-json-pretty>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from "vue";
import VueJsonPretty from "vue-json-pretty";
import "vue-json-pretty/lib/styles.css";

const jsonInput = ref("");
const jsonData = ref(null);
const errorMessage = ref("");

const isValidJson = computed(() => {
  try {
    JSON.parse(jsonInput.value);
    return true;
  } catch {
    return false;
  }
});

const updateJsonData = () => {
  try {
    jsonData.value = JSON.parse(jsonInput.value);
    errorMessage.value = "";
  } catch (error) {
    jsonData.value = null;
    errorMessage.value = "无效的 JSON 格式";
  }
};

const formatJson = () => {
  try {
    const formatted = JSON.stringify(JSON.parse(jsonInput.value), null, 2);
    jsonInput.value = formatted;
    updateJsonData();
  } catch (error: any) {
    errorMessage.value = "无法格式化 JSON：" + error.message;
  }
};

const clearJson = () => {
  jsonInput.value = "";
  jsonData.value = null;
  errorMessage.value = "";
};

const handleClick = (path: any, data: any) => {
  console.log("Clicked path:", path);
  console.log("Clicked data:", data);
};
</script>

<style scoped>
.json-viewer-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

.el-input {
  margin-bottom: 15px;
}

.el-button {
  margin-right: 10px;
  margin-bottom: 15px;
}

.error-message {
  color: #f56c6c;
  margin-bottom: 15px;
}

.json-view-wrapper {
  border: 1px solid #dcdfe6;
  border-radius: 4px;
  padding: 10px;
  overflow: auto;
  max-height: 500px;
}
</style>
