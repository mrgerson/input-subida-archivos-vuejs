<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card">
                    <div class="card-header">Laravel Vue JS File Upload Demo</div>
                    <h1>{{resultado}}</h1>

                    <div class="card-body">

                        <div v-if="success != ''" class="alert alert-success">
                            success
                        </div>

                        <form @submit="formSubmit" enctype="multipart/form-data">
                            <input type="file" class="form-control" id="file" v-on:change="onChange">
                            <button class="btn btn-primary btn-block">Upload</button>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data(){
            return {
                name: '',
                file: '',
                success: '',
                resultado: ''
            };
        },

        methods: {
            onChange(e){
                this.file = e.target.files[0];
            },
            formSubmit(e){
                e.preventDefault();
                let existingObj = this;

                let data = new FormData();
                data.append('file', this.file);

               // console.log(data)
                console.log( data.get('file') );



                 let fileInput = document.getElementById('file');
                 let filePath = fileInput.value;
                 let allowedExtensions = /(.jpg|.jpeg|.png|.gif)$/i;
                 if(!allowedExtensions.exec(filePath)){
                    alert('cargue un archivo que tenga extensiones .jpeg/.jpg/.png/.gif only.');
                    fileInput.value = '';
                    return false;
                 }else{
                    //Image preview
                    if (fileInput.files && fileInput.files[0]) {
                        var reader = new FileReader();
                         reader.onload = function(e) {
                            //document.getElementById('imagePreview').innerHTML = '<img src="'+e.target.result+'"/>';
                           // console.log(e.target.result)
                           console.log("archivo cargado con exito")
                            
                        }; 
                        reader.readAsDataURL(fileInput.files[0]);
                    }
                }

                /*  axios.post('/upload', data, 
                 {
                    headers: {
                      "Content-Type": "multipart/form-data"
                    } 
                 })
                  .then(function (res){
                      existingObj.success = res.data.success;
                  })
                  .catch(function (err){
                      this.existingObj.output = err;
                  });  */
            },


        }
    }

</script>
