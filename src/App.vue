<script setup lang="ts">
import Upload from "./components/Upload.vue";
import Add from "./components/Add.vue";
import JsonViewer from "./components/JsonViewer.vue";
import DamageTypeSetter from "./components/DamageTypeSetter.vue";
import SingleImageDisplay from "./components/SingleImageDisplay.vue";
import { ref, provide } from "vue";
import localImage from "./assets/demo3.jpg";

const key = ref("");
const result = ref(null);

const updateKey = (val: any) => {
  key.value = val;
};

const updateResult = (val: any) => {
  result.value = val;
};

provide("key", {
  key,
  updateKey,
});

provide("result", {
  result,
  updateResult,
});

const imageUploadRef = ref(null) as any;
const damageTypeSetterRef = ref(null) as any;
const SingleImageDisplayRef = ref(null) as any;

const handleParentReselect = () => {
  if (imageUploadRef.value) {
    imageUploadRef.value.handleReselect();
  }
};

const handleParentUpload = () => {
  if (imageUploadRef.value) {
    imageUploadRef.value.submitUpload();
  }
};

const handleGenerateCard = () => {
  // @ts-ignore
  ElMessage.success("生成成功");
  SingleImageDisplayRef.value.updateImageUrl(localImage);
};
</script>

<template>
  <header class="navbar">
    <div class="navbar-wrapper">
      <div class="logo-container">
        <img class="logo" src="./assets/logo.svg" alt="logo" />
        <h1 class="title">自动生成伤害卡</h1>
      </div>
    </div>
  </header>
  <div class="container">
    <div class="content">
      <div class="upload">
        <div class="upload-header-container">
          <div class="upload-header">
            <div class="upload-title">上传病历附件</div>
            <el-button type="primary" @click="handleParentReselect"
              >重新选择</el-button
            >
          </div>
        </div>
        <div class="upload-body">
          <Upload ref="imageUploadRef" />
        </div>
      </div>
      <div class="result">
        <div class="upload-header-container">
          <div class="upload-header">
            <div class="upload-title">抽取病历结果</div>
            <el-button type="primary" @click="handleParentUpload"
              >抽取字段</el-button
            >
          </div>
        </div>
        <div class="result-config">
          <div class="result-config-body">
            <Add />
          </div>
        </div>
        <div class="result-body">
          <JsonViewer />
        </div>
      </div>
      <div class="generate">
        <div class="upload-header-container">
          <div class="upload-header">
            <div class="upload-title">生成伤害卡</div>
            <el-button type="primary" @click="handleGenerateCard"
              >生成卡片</el-button
            >
          </div>
        </div>
        <div>
          <DamageTypeSetter ref="damageTypeSetterRef" />
        </div>
        <div class="generate-title">伤害卡：</div>
        <div class="generate-body">
          <SingleImageDisplay ref="SingleImageDisplayRef" />
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.navbar {
  position: sticky;
  top: 0;
  left: 0;
  z-index: 12;
  width: 100%;
}

.navbar-wrapper {
  position: relative;
  border-bottom: 1px solid #dcdfe6;
  height: 55px;
  padding: 0 32px;
  background-image: radial-gradient(transparent 1px, #fff 1px);
  background-size: 4px 4px;
  backdrop-filter: saturate(50%) blur(4px);
  -webkit-backdrop-filter: saturate(50%) blur(4px);
  top: 0;
}

.logo-container {
  display: flex;
  align-items: center;
  height: 100%;
}

.logo {
  height: 32px;
}

.title {
  margin-left: 8px;
  font-size: 18px;
  font-weight: 700;
  color: #409eff;
}

.container {
  display: flex;
  padding: 20px 20px 55px;
  flex: 1;
  box-shadow: 4px 4px 10px rgba(0, 0, 0, 0.1);
}

.content {
  display: flex;
  flex: 1;
  background-color: #fff;
  border-radius: 8px;
}

.content > div {
  flex: 1;
}

.upload,
.result {
  border-right: 1px solid #e8e8e8;
}

.upload-header-container {
  padding: 0 16px;
}

.upload-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 56px;
  border-bottom: 1px solid #e8e8e8;
}

.upload-title {
  font-size: 16px;
  font-weight: 700;
}

.upload {
  display: flex;
  flex-direction: column;
}

.upload-body {
  display: flex;
  flex: 1;
  padding: 20px 20px 40px;
  flex-direction: column;
}

.result {
  display: flex;
  flex-direction: column;
}

.result-config {
  padding: 20px 20px 0;
}

.result-body {
  display: flex;
  flex: 1;
}

.result-config-body {
  border-bottom: 1px solid #e8e8e8;
}

.generate {
  display: flex;
  flex-direction: column;
}

.generate-body {
  display: flex;
  flex: 1;
  padding: 0 20px 40px;
}

.generate-title {
  font-size: 14px;
  padding-left: 20px;
  margin-bottom: 10px;
}
</style>
