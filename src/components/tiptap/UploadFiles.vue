<template>
  <div class="upload-container">
    <el-upload
      ref="uploadFile"
      :limit="limit"
      :action="action"
      :headers="{
        Authorization: Authorization,
      }"
      :data="{ storeId: storeId }"
      :file-list="fileList"
      :on-remove="handleRemove"
      :on-error="handleError"
      :on-success="handleSuccess"
      :on-exceed="handleExceed"
      :on-change="handleChange"
      :before-upload="beforeUpload"
      :auto-upload="autoUpload"
      :show-file-list="showFileList"
    >
    <slot></slot> 
    </el-upload>
  </div>
</template>
<script>
export default {
  data() {
    return {
      waiting: false,
      fileList: [],
    };
  },
  props: {
    limit: {
      type: Number,
      default: 1,
    },
    action: {
      default: '',
      type: String,
    },
    Authorization: {
      default: localStorage.getItem('loginToken'),
      type: String,
    },
    storeId: {
      default: localStorage.getItem('store_id'),
      type: String,
    },
    limitMB: {
      default: 5,
      type: Number,
    },
    type: {
      default: () => [],
      type: Array,
    },
    autoUpload: {
      type: Boolean,
      default: false,
    },
    showFileList: {
      type: Boolean,
      default: false,
    },
  },
  mounted() {},
  methods: {
    handleRemove() {
      this.clearFiles();
    },
    handleError() {
      this.$message.error('上传失败');
    },
    handleSuccess(e) {
      this.$message.success('上传成功');
      this.$emit('upload', e.data);
      this.clearFiles();
    },
    handleExceed() {
      this.$message.warning('已超过上传数量限制');
    },
    handleChange() {
      // this.waiting = true;
    },
    beforeUpload(file) {
      const currentFileType = file.type;
      const isTypeFile = this.type.indexOf(currentFileType) > -1;
      const isLtMB = file.size / 1024 / 1024 < this.limitMB;
      if (!isTypeFile) {
        let typeMsg = '';
        this.type.forEach((t, index) => {
          typeMsg += t.split('/')[1];
          if (index < this.type.length) {
            typeMsg += '/';
          }
        });
        this.$message.warning('上传文件格式必须为' + typeMsg);
      }
      if (!isLtMB) {
        this.$message.warning('上传图片大小不能超过 200MB');
      }
      return isTypeFile && isLtMB;
    },
    submitUpload() {
      console.log(this.fileList);
      this.$refs.uploadFile.submit();
    },
    clearFiles() {
      this.$refs.uploadFile.clearFiles();
    },
  },
};
</script>
<style lang="scss" scoped>
.trigger {
  display: flex;
  justify-content: space-around;
  margin-top: 20px;
}
.uploadBar {
  display: flex;
  flex-direction: column;
  justify-content: center;
}
</style>
