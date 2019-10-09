<template>
    <div>
        <button type="button" class="button is-info" @click="save">{{button_name}}</button>

        <div class="modal" v-bind:class="{'is-active':showModal}">
            <div class="modal-background"></div>
            <div class="modal-card">
                <header class="modal-card-head">
                    <p class="modal-card-title">{{modal_title}}</p>
                    <button type="button" class="delete" aria-label="close" @click="closeAction"></button>
                </header>
                <section class="modal-card-body">
                    {{modal_text}}
                </section>
                <footer class="modal-card-foot">
                    <button type="button" class="button is-success" v-show="is_save">更新</button>
                    <button type="button" class="button" @click="closeAction">閉じる</button>
                </footer>
            </div>
        </div>

    </div>

</template>
<script>
    export default {
        data: function () {
            return {
                showModal   : false,
                modal_text  : '',
            }
        },
        props: ['button_name', 'product_id', 'is_save', 'modal_title'],
        // props: {
        //     button_name : String,
        //     product_id  : String,
        //     is_save     : Boolean,
        //     modal_title : String
        // },
        methods: {
            save: function() {

                var self = this;
                let data = {
                    product_id: this.product_id
                };

                axios.post( '/creator/product/apply', data
                ).then(function(response){
                    self.showModal = true;
                    self.modal_text = '申し込みを行いました。';
                }).catch(function(error){
                    self.showModal = true;
                    self.modal_text = '申し込みに失敗しました。';
                });
            },
            closeAction: function() {
                this.showModal = false;
                window.location.href = "/creator/product/";
            }
        },
    }
</script>
