<template>
  <f7-page name="home">
    <f7-navbar :sliding="false">
      <f7-nav-title>runway_strip</f7-nav-title>
    </f7-navbar>

    <f7-list inline-labels no-hairlines-md class="list">
      <f7-list-item
          title="Файл"
      >
        <template #after>
          <input
              id="inputFile"
              type="file"
              required
              class="hide-input file"
              ref="inputFile"
              @change="handleInputFile"
              :disabled="isLoading"
          />
          <label for="inputFile" class="cursor-pointer label-file">
            {{ nameFile }}
            <f7-icon material="upload_file"/>
          </label>
        </template>
      </f7-list-item>
      <f7-button
          type="submit"
          class="mt10"
          fill
          preloader
          :loading="isLoading"
          :disabled="isDisabled || isLoading"
          @click="loadFileData">Просчитать
      </f7-button>
    </f7-list>
  </f7-page>
</template>

<script>
import readXlsxFile from 'read-excel-file'
import {f7, f7ready} from "framework7-vue";

export default {
  data() {
    return {
      inputFile: null,
      isLoading: false,
      isDisabled: true,
      nameFile: 'Файл не выбран',
      fileData: []
    }
  },

  mounted() {
    f7ready(() => {
      if (this.$refs.inputFile) {
        this.inputFile = this.$refs.inputFile;
      }
      // f7.dialog.alert('Hello', () => {
      //   f7.loginScreen.close();
      // });
    });
  },

  methods: {
    loadFileData() {
      this.isLoading = true;
      if (this.inputFile.files.length) {
        readXlsxFile(this.inputFile.files[0]).then((rows) => {
          // this.isLoading = false;
          this.workFileData(rows)
        })
      } else {
        this.isLoading = false;
      }
    },

    handleInputFile() {
      this.isDisabled = !this.inputFile.files.length;
      this.nameFile = this.inputFile.files[0]?.name || 'Файл не выбран'
    },

    /**
     * @param {Array} data
     */
    workFileData(data) {
      const index = 5;
      this.fileData = data.slice(index, data.length);
      console.log(this.fileData)
    }
  }
}
</script>

<style scoped lang="scss">
.list {
  max-width: 600px;
  margin: 50px auto;
}

.label-file {
  transition: 0.3s;

  &:hover {
    color: red;
  }
}

.file:valid + .label-file {
  color: red;
}
</style>