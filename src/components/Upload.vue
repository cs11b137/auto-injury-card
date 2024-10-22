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
      :before-upload="beforeUpload"
      ref="uploadRef">
      <img v-if="imageUrl" :src="imageUrl" class="avatar" alt="" />
      <el-icon v-else class="avatar-uploader-icon">
        <upload-filled />
        <div class="upload__text">将文件拖到此处或点击上传</div>
      </el-icon>
    </el-upload>
  </div>
</template>

<script setup lang="ts">
import { ref, defineExpose, inject } from "vue";
import { UploadFilled } from "@element-plus/icons-vue";
import axios from "axios";
import { ElMessage, type UploadRawFile } from "element-plus";

const { key } = inject("key") as any;
const { updateResult } = inject("result") as any;

const imageUrl = ref("");
const uploadRef = ref(null) as any;
const currentFile = ref<UploadRawFile | null>(null);

// 文件验证
const beforeUpload = (file: File) => {
  // 检查文件类型
  const allowedTypes = ["image/png", "image/jpeg", "image/jpg"];
  const isValidType = allowedTypes.includes(file.type);

  if (!isValidType) {
    ElMessage.error("只支持 PNG、JPG、JPEG 格式的图片！");
    return false;
  }

  // 检查文件大小（50MB）
  const maxSize = 50 * 1024 * 1024;
  if (file.size > maxSize) {
    ElMessage.error("文件大小不能超过 50MB！");
    return false;
  }

  // 如果是图片，检查尺寸
  if (file.type.startsWith("image/")) {
    return new Promise((resolve, reject) => {
      const img = new Image();
      img.src = URL.createObjectURL(file);
      img.onload = () => {
        if (
          img.width >= 20 &&
          img.width <= 10000 &&
          img.height >= 20 &&
          img.height <= 10000
        ) {
          resolve(true);
        } else {
          ElMessage.error("图片尺寸必须在 20x20 到 10000x10000 之间！");
          reject(false);
        }
      };
    });
  }

  return true;
};

const handleChange = (uploadFile: any) => {
  if (uploadFile && uploadFile.raw) {
    imageUrl.value = URL.createObjectURL(uploadFile.raw);
    currentFile.value = uploadFile.raw;
  } else {
    ElMessage.error("文件选择失败，请重试");
    currentFile.value = null;
  }
};

const uploadImage = async (file: File) => {
  try {
    // 读取文件为二进制数据
    const reader = new FileReader();
    reader.readAsArrayBuffer(file);

    reader.onload = async () => {
      try {
        const response = await axios.post(
          "https://api.textin.com/ai/service/v1/entity_extraction",
          reader.result,
          {
            headers: {
              "Content-Type": "application/octet-stream",
              "x-ti-app-id": "8234d5965139e24f0260db7e25844508", // 需要替换为实际的 app-id
              "x-ti-secret-code": "bbba79ab9c8908a094b09dd9c3ea8949", // 需要替换为实际的 secret-code
            },
            params: {
              key: key.value, // 根据需要设置key值
            },
          }
        );

        ElMessage({
          type: "success",
          message: "文档处理成功",
        });
        console.log(response.data);
        updateResult(response.data);
      } catch (error: any) {
        ElMessage.error(error.response?.data?.message || "处理失败");
        console.error(error);
      }
    };

    reader.onerror = () => {
      ElMessage.error("文件读取失败");
    };
  } catch (error) {
    ElMessage.error("上传失败");
    console.error(error);
  }
};

const handleReselect = () => {
  imageUrl.value = "";
  if (uploadRef.value) {
    uploadRef.value.clearFiles();
  }
};

const submitUpload = () => {
  if (currentFile.value) {
    uploadImage(currentFile.value);
  } else {
    ElMessage.warning("请先选择一张图片");
  }
};

defineExpose({
  handleReselect,
  submitUpload,
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
