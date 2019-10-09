<template>
    <div class="field">
        <div class="file is-info has-name">
            <label class="file-label">
                <input class="file-input" type="file" id="file" name="product_image" @change="onFileChange" >
                <span class="file-cta">
                <span class="file-icon">
                  <i class="fas fa-upload"></i>
                </span>
                <span class="file-label">
                  Info file…
                </span>
              </span>
                <span class="file-name" style="max-width: none; !important;">
                {{s3name}}
              </span>
            </label>
        </div>

        <div class="file is-info has-name" style="margin-top:10px;">
            <img :src="s3product_image" width="200px" alt="">
        </div>

        <div class="file is-info has-name" style="margin-top:10px;">
            <a class="button is-primary" v-on:click="upload_product_image">アップロード</a>
        </div>

    </div>
</template>
<script>
    export default {
        data: function () {
            return {
                s3name : this.product_image,
                s3product_image : '/s3/get/' + this.product_image
            }
        },
        props: {
            product_image : String,
            product_id: String
        },
        methods: {
            onFileChange: function(event) {
                this.file = event.target.files || event.dataTransfer.files;
                this.s3name = this.file[0].name;
            },
            upload_product_image: function(){
                let formData = new FormData();

                formData.append('file', this.file[0]);
                formData.append('product_id', this.product_id);

                var self = this;
                axios.post( '/creator/product/file_upload',
                    formData,
                    {
                        headers: {
                            'Content-Type': 'multipart/form-data'
                        }
                    }
                ).then(function(response){
                    alert('正常に保存されました。');
                    self.s3product_image = '/s3/get/' + response.data.path;
                })
                .catch(function(error){
                    alert('保存に失敗しました。');
                    console.log(error);
                });
            }
        },
    }
</script>
