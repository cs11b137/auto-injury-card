<template>
  <div class="json-viewer-container">
    <div class="json-input-title">JSON结果：</div>
    <div v-if="errorMessage" class="error-message">{{ errorMessage }}</div>
    <div class="json-view-wrapper" v-if="jsonData">
      <vue-json-pretty
        :data="jsonData"
        :deep="1"
        :show-double-quotes="true"
        :show-length="true"
        @click="handleClick"
      ></vue-json-pretty>
    </div>
    <div class="json-view-wrapper json-empty" v-else>
      <el-empty :image-size="100" description="暂无结果" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, inject } from "vue";
import VueJsonPretty from "vue-json-pretty";
import "vue-json-pretty/lib/styles.css";

const { result } = inject("result") as any;

const jsonData = ref(result);
const errorMessage = ref("");

const handleClick = (path: any, data: any) => {
  console.log("Clicked path:", path);
  console.log("Clicked data:", data);
};
</script>

<style scoped>
.json-viewer-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px 20px 40px;
  display: flex;
  flex: 1;
  flex-direction: column;
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
  max-height: 800px;
  flex: 1;
}

.json-input-title {
  font-size: 14px;
  margin-bottom: 10px;
}

.json-empty {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
