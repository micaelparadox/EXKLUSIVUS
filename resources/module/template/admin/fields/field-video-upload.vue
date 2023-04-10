<!-- field-video-upload.vue -->
<template>
    <div class="video-upload-box" :class="{ 'active': value }" style="width: 200px">
        <div class="upload-box" v-show="!value">
            <div class="text-center">
                <label for="video-upload">{{ field.label }}</label>
            </div>
            <div class="text-center">
                <span class="btn btn-primary" @click="openUploader">Upload video</span>
                <input id="video-upload" type="file" :name="field.name" @change="onFileSelected" accept="video/*"
                    ref="videoInput" style="display: none" />
            </div>
        </div>
        <div class="video-demo" title="Change file" @click="openUploader" v-show="value">
            <video :src="previewUrl" controls class="video-responsive"></video>
        </div>
        <div class="upload-actions justify-content-between" v-show="value">
            <a href="#" @click.prevent="removeVideo">Remove</a>
        </div>
        <span class="help-block">{{ field.help }}</span>
    </div>
</template>

<script>
export default {
    name: "fieldVideoUpload",
    props: {
        field: {
            type: Object,
            required: true,
        },
        model: {
            type: Object,
            required: true,
        },
    },
    data() {
        return {
            value: null,
            previewUrl: null,
        };
    },
    methods: {
        openUploader() {
            this.$refs.videoInput.click();
        },
        onFileSelected(event) {
            const file = event.target.files[0];
            if (file) {
                this.value = file;
                this.previewUrl = URL.createObjectURL(file);
                this.$emit("input", {
                    field: this.field,
                    value: file,
                });
            }
        },
        removeVideo() {
            this.value = null;
            this.previewUrl = null;
            this.$emit("input", {
                field: this.field,
                value: null,
            });
        },
    },
};
</script>

<style scoped>
.video-upload-box {
    position: relative;
    border: 1px solid #ccc;
    padding: 10px;
    border-radius: 5px;
}

.video-responsive {
    max-width: 100%;
    height: auto;
}

.upload-actions {
    display: flex;
    position: absolute;
    bottom: 10px;
    width: 100%;
    padding: 0 10px;
}
</style>
