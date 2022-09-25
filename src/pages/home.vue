<template>
  <f7-page name="home">
    <!-- Top Navbar -->
    <f7-navbar :sliding="false">
      <f7-nav-title>runway_strip</f7-nav-title>
    </f7-navbar>
    <!-- Toolbar-->


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

export default {
  data() {
    return {
      inputFile: null,
      isLoading: false,
      isDisabled: true,
      nameFile: 'Файл не выбран'
    }
  },
  mounted() {
    if (this.$refs.inputFile) {
      this.inputFile = this.$refs.inputFile;
    }
  },

  methods: {
    loadFileData() {
      this.isLoading = true;
      readXlsxFile(this.inputFile.files[0]).then((rows) => {
        // this.isLoading = false;
        this.workFileData(rows)
      })
    },

    handleInputFile() {
      this.isDisabled = !this.inputFile.files.length;
      this.nameFile = this.inputFile.files[0]?.name || 'Файл не выбран'
    },

    workFileData(data) {
      console.log(data)
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