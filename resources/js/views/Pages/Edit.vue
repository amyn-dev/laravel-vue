<template>
    <div>
        <div class="card mb-3">
            <div class="card-header d-flex">
                {{ fields.title }}
                <router-link exact :to="{name: 'pages'}" class="ml-auto btn btn-danger btn-sm">
                    {{ $t('actions.cancel') }}
                </router-link>
            </div>
            <div class="card-body">
                <div class="container">
                    <form @submit.prevent="updatePage" enctype="multipart/form-data">

                        <div class="col-lg-5 form-group">
                            <label> {{ $t('fields.title') }} </label>
                            <input type="text" class="form-control" v-model="fields.title">
                            <div v-if="errors && errors.title">
                                <div v-for="error in errors.title"
                                     class="text-danger" role="alert">
                                    <p>{{ error }}</p>
                                </div>
                            </div>
                        </div>

                        <div class="col-lg-5 form-group">
                            <label> {{ $t('fields.url') }} </label>
                            <input type="text" class="form-control" v-model="fields.slug">
                            <div v-if="errors && errors.slug">
                                <div v-for="error in errors.slug"
                                     class="text-danger" role="alert">
                                    <p>{{ error }}</p>
                                </div>
                            </div>
                        </div>

                        <div class="col-lg-12 form-group">
                            <label> {{ $t('fields.content') }} </label>

                            <ckeditor :editor="editor" v-model="fields.content" :config="editorConfig"></ckeditor>
                            <div v-if="errors && errors.content">
                                <div v-for="error in errors.content"
                                     class="text-danger" role="alert">
                                    <p>{{ error }}</p>
                                </div>
                            </div>
                        </div>

                        <div class="col-lg-12 form-group">
                            <button type="submit" class="btn btn-primary mt-3">{{ $t('buttons.update') }}</button>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import ClassicEditor from '@ckeditor/ckeditor5-build-classic';
import {useRoute, useRouter} from "vue-router";
import {useI18n} from "vue-i18n/index";
import {reactive, toRefs} from "vue";

export default {
    setup() {
        const i18n = useI18n()
        const router = useRouter()
        const route = useRoute()
        const state = reactive({
            fields: {
                title: "",
                slug: "",
                content: ""
            },
            editor: ClassicEditor,
            editorConfig: {
                // toolbar: [ 'bold', 'italic', '|', 'link', 'undo', 'redo'],
                language: 'ar'
            },
            editorDisabled: true,
            errors: []
        })

        function loadPage() {
            axios.get(`/api/v1/pages/${route.params.id}`).then(response => {
                state.fields.title = response.data.page.title
                state.fields.slug = response.data.page.slug
                state.fields.content = response.data.page.content
            })
        }

        loadPage()

        function updatePage() {
            axios.put(`/api/v1/pages/${route.params.id}`, state.fields).then(response => {
                toast.fire({
                    icon: 'success',
                    title: i18n.t('messages.updated_successfully')
                })
                router.push({name: 'pages'})
            }).catch(error => {
                state.errors = error.response.data.errors;
            })
        }

        return {
            state,
            updatePage,
            ...toRefs(state)
        }
    }
}

</script>

<style scoped></style>
