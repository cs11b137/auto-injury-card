<template>
  <div class="image-upload">
    <el-upload
      class="avatar-uploader"
      action="#"
      drag
      :show-file-list="false"
      :on-change="handleChange"
      :auto-upload="false"
      :limit="1"
      :http-request="uploadImage"
      ref="uploadRef"
    >
      <img v-if="imageUrl" :src="imageUrl" class="avatar" alt="" />
      <el-icon v-else class="avatar-uploader-icon">
        <upload-filled />
        <div class="upload__text">将文件拖到此处或点击上传</div>
      </el-icon>
    </el-upload>
  </div>
</template>

<script setup lang="ts">
import { ref, defineExpose } from "vue";
import { UploadFilled } from "@element-plus/icons-vue";
import axios from "axios";
import { ElMessage } from "element-plus";

const imageUrl = ref("");
const uploadRef = ref(null) as any;

const handleChange = (file: any) => {
  if (file && file.raw) {
    imageUrl.value = URL.createObjectURL(file.raw);
  } else {
    ElMessage.error("文件选择失败，请重试");
  }
};

const uploadImage = async (options: any) => {
  try {
    const formData = new FormData();
    formData.append("image", options.file);

    const response = await axios.post("/api/upload", formData, {
      headers: {
        "Content-Type": "multipart/form-data",
      },
    });

    ElMessage.success("图片上传成功");
    console.log(response.data);
  } catch (error) {
    ElMessage.error("上传失败");
    console.error(error);
  }
};

const handleReselect = () => {
  console.log(1234);
  imageUrl.value = "";
  if (uploadRef.value) {
    uploadRef.value.clearFiles();
  }
};

const submitUpload = () => {
  if (uploadRef.value && uploadRef.value.uploadFiles.length > 0) {
    uploadImage({ file: uploadRef.value.uploadFiles[0].raw });
  } else {
    ElMessage.warning("请先选择一张图片");
  }
};

defineExpose({
  handleReselect,
});
</script>

<style scoped>
.image-upload {
  display: flex;
  flex-direction: column;
  align-items: center;
  flex: 1;
}

.avatar-uploader {
  display: flex;
  width: 100%;
  flex: 1;
}

.avatar-uploader .avatar {
  width: auto;
  height: auto;
  display: block;
  max-width: 80%;
}

.avatar-uploader .el-upload {
  border: 1px dashed var(--el-border-color);
  border-radius: 6px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
  transition: var(--el-transition-duration-fast);
}

.avatar-uploader .el-upload:hover {
  border-color: var(--el-color-primary);
}

.avatar-uploader-icon {
  display: flex;
  flex-direction: column;
  font-size: 28px;
  color: #8c939d;
  width: auto;
  height: 178px;
  text-align: center;
}

.avatar-uploader-icon svg {
  width: 100px;
  height: 100px;
}

.button-container {
  margin-top: 15px;
  display: flex;
  gap: 10px;
}

.upload__text {
  font-size: 16px;
}
</style>
