<template>
  <div class="about">
    <div id="myapp">
      <div class="row">
        <div class="col-md-6">
          <div class="input-group">
            <div class="custom-file mb-3"> 
              <input type="file" class="custom-file-input" id="uploadfiles" ref="uploadfiles"  required />
              <label class="custom-file-label" for="uploadfiles">Elija varias archivos para cargar</label>
            </div>
          </div>
        </div>
        <div class="col-md-2">
          <button type="button" @click="uploadFile()" name="upload" class="btn btn-primary">Cargar Archivos</button>
        </div>
      </div> 
      <hr>
      <!-- Mostrando los archivos cargados -->
      <div v-if="filenames.length" >
        <ul>
            <li v-for= "(filename,index) in filenames" :key="index"> {{ filename }} </li>
       </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return{
      file: "",
      filenames: []
    };
  },
    
  methods: {

    uploadFile: function(){
        var el = this;
        let formData = new FormData();
        // Leer archivos seleccionados
        var files = this.$refs.uploadfiles.files;
        var totalfiles = this.$refs.uploadfiles.files.length;
        for (var index = 0; index < totalfiles; index++) {
          formData.append("files[]", files[index]);
        }

        this.filenames = formData

        console.log(formData)

        /*  axios.post("ajaxfile.php", formData,
        {
          headers: {
            "Content-Type": "multipart/form-data"
          }
        })
        .then(function (response) {
          el.filenames = response.data;
          alert(el.filenames.length + " los archivos se han subido.");
        })
        .catch(function (error) {
          console.log(error);
        });  */
    }
  }

}
</script>
