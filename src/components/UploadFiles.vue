<template>
  <div class="upload-container">
    <el-upload
      ref="uploadFile"
      :limit="limit"
      :action="action"
      :headers="headers"
      :data="params"
      :file-list="fileList"
      :on-remove="handleRemove"
      :on-error="handleError"
      :on-success="handleSuccess"
      :on-exceed="handleExceed"
      :on-change="handleChange"
      :before-upload="beforeUpload"
      :auto-upload="true"
      :show-file-list="false"
      :accept="accept"
    >
    <slot></slot> 
    </el-upload>
  </div>
</template>
<script>
export default {
  data() {
    return {
      fileList: [],
    };
  },
  props: {
    limit: {
      default: 1,
      type: Number,
    },
    action: {
      type: String,
      default: '',
    },
    headers:  {
      type: Object,
      default: () => {},
    },
    limitMB: {
      type: Number,
      default: 5,
    },
    accept: {
      type: String,
      default: '',
    },
    params: {
      type: Object,
      default: () => {},
    },
  },
  mounted() {},
  methods: {
    handleRemove() {
      this.clearFiles();
    },
    handleError() {
      this.$message.error('upload falsed');
    },
    handleSuccess(e) {
      this.$message.success('upload sussess');
      this.$emit('upload', e);
      this.clearFiles();
    },
    handleExceed() {
      this.$message.warning('can not upload more than' + this.limit);
    },
    handleChange() {},
    beforeUpload(file) {
      // const currentFileType = file.type;
      // const isTypeFile = this.type.length > 0 ? this.type.indexOf(currentFileType) > -1 : true;
      const isLtMB = file.size / 1024 / 1024 < this.limitMB;
      // if (!isTypeFile) {
      //   let typeMsg = '';
      //   this.type.forEach((t, index) => {
      //     typeMsg += t.split('/')[1];
      //     if (index < this.type.length) {
      //       typeMsg += '/';
      //     }
      //   });
      //   this.$message.warning('upload type must be' + typeMsg);
      // }
      if (!isLtMB) {
        this.$message.warning('upload limit size is' + this.limitMB + 'MB');
      }
      return isLtMB;
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
