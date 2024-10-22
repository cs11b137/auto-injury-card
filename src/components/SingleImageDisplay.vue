<template>
  <div class="single-image-container">
    <el-image
      v-if="imageUrl"
      :src="imageUrl"
      :preview-src-list="[imageUrl]"
      fit="contain"
      :initial-index="0"
      class="image">
      <!-- 加载中显示骨架屏 -->
      <template #placeholder>
        <div class="image-placeholder">
          <el-skeleton animated>
            <template #template>
              <el-skeleton-item
                variant="image"
                style="width: 100%; height: 100%" />
            </template>
          </el-skeleton>
        </div>
      </template>

      <!-- 加载失败显示图标 -->
      <template #error>
        <div class="image-error">
          <el-icon><Picture /></el-icon>
          <span>加载失败</span>
        </div>
      </template>
    </el-image>
    <el-empty :image-size="100" description="暂无报告卡" v-else />
  </div>
</template>

<script setup lang="ts">
import { Picture } from "@element-plus/icons-vue";
import { ref, defineExpose } from "vue";

// 可以通过props接收图片URL，这里使用默认值作为示例

const imageUrl = ref(null) as any;

const updateImageUrl = (url: string) => {
  imageUrl.value = url;
};

defineExpose({
  updateImageUrl,
});
</script>

<style scoped>
.single-image-container {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  max-width: 800px;
  margin: 0 auto;
  background-color: #f5f7fa;
  border-radius: 4px;
  overflow: hidden;
}

.image {
  width: 100%;
  height: 300px;
  display: block;
  cursor: pointer;
}

.image-placeholder {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #f5f7fa;
}

.image-error {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  color: #909399;
  font-size: 14px;
  gap: 8px;
}

.image-error .el-icon {
  font-size: 24px;
}
</style>
