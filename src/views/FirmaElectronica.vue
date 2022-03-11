<template>
  <el-upload
    multiple
    class="upload-demo"
    drag
    ref="upload"
    :data="demandform"
    action="http://186.1.181.82/SIMinTics/backend/public/api/auth/demands"
    :file-list="annexes"
    :on-preview="handlePreview"
    :on-change="handleSuccess"
    :auto-upload="false"
  >
    <i class="el-icon-upload"></i>
    <div class="el-upload__text">
      Suelta tus documentos aquí o <em>haz clic para cargar</em>
    </div>
    <div slot="tip" class="el-upload__tip">
      Solo archivos
      <span style="color: #409eff"
        >.pdf / .excel / .word / .jpg / .png / .zip / .rar</span
      >
    </div>
  </el-upload>
</template>

<script>
export default {
  data() {
    return {};
  },
  computed: {},
  methods: {
    handlePreview(file) {
      this.$emit("cotentLoading", true);
      if (file.status == "success") {
        axios({
          url: file.url,
          method: "GET",
          responseType: "blob",
        }).then((response) => {
          const url = window.URL.createObjectURL(new Blob([response.data]));
          const link = document.createElement("a");
          link.href = url;
          link.setAttribute("download", file.name);
          document.body.appendChild(link);
          link.click();
          this.$emit("cotentLoading");
        });
      } else {
        const url = window.URL.createObjectURL(new Blob([file.raw]));
        const link = document.createElement("a");
        link.href = url;
        link.setAttribute("download", file.name);
        document.body.appendChild(link);
        link.click();
        this.$emit("cotentLoading");
      }
    },

    handleSuccess(file, annexes) {
      if (localStorage.getItem("demandEdit")) {
        this.annexes.forEach((annex) => {
          if (annex.name == file.raw.name) {
            this.$refs.upload.handleRemove(file);
            showSwalAlert(
              "El archivo " +
                file.raw.name +
                " ya esta cargado en la lista de archivos.",
              "warning"
            );
            return false;
          }
        });
      }

      const filesRaw = this.$refs.upload.uploadFiles.map(
        (annexe) => annexe.raw
      );
      let size = filesRaw.length - 1;
      for (let index = 0; index < size; index++) {
        const element = filesRaw[index];
        if (element && element.name == file.raw.name) {
          this.$refs.upload.handleRemove(element);
          showSwalAlert(
            "El archivo " +
              file.raw.name +
              " ya esta cargado en la lista de archivos.",
            "warning"
          );
          return false;
        }
      }

      let types = [
        "image/jpeg",
        "image/png",
        "application/pdf",
        "application/msword",
        "application/vnd.ms-excel",
        "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet",
        "application/vnd.openxmlformats-officedocument.wordprocessingml.document",
        "application/zip",
        "application/x-zip-compressed",
        "application/x-rar-compressed",
      ];

      if (!file.name.includes(".rar") && !types.includes(file.raw.type)) {
        console.log(file.raw.type);
        showSwalAlert(
          "Lo sentimos, ha seleccionado un archivo no valido, la extensión debe ser .pdf / .excel / .word / .jpg / .png / .zip / .rar!",
          "warning"
        );
        this.$refs.upload.handleRemove(file);
        return false;
      }

      return true;
    },
  },
  created() {},
};
</script>

<style></style>
