<template>
  <div class="hello">
    <input
    style="display: none"
    type="file"
    @change="onFileSelected"
    ref="fileInput">
    <button @click="$refs.fileInput.click()">Select File</button>
    <button @click="onUpload">Upload</button>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'HelloWorld',
  data() {
    return {
      selectedFile: null
    };
  },
  methods: {
    onFileSelected(event) {
      this.selectedFile = event.target.files[0];
    },
    onUpload() {
      const fd = new FormData();
      fd.append('image', this.selectedFile, this.selectedFile.name);
      axios
        .post(
          'https://us-central1-fb-cloud-functions-demo-fefa9.cloudfunctions.net/uploadFile ',
          fd,
          {
            onUploadProgress: uploadEvent => {
              console.log(
                'Upload progress: ' +
                  Math.round(uploadEvent.loaded / uploadEvent.total * 100) +
                  '%'
              );
            }
          }
        )
        .then(res => {
          console.log(res);
        });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
