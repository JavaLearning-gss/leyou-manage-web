<template>
  <v-dialog max-width="500" v-model="show" persistent>
    <v-card>
      <!--对话框的标题-->
      <v-toolbar dense dark color="primary">
        <v-toolbar-title>新增品牌</v-toolbar-title>
      </v-toolbar>
      <!--对话框的内容，表单-->
      <v-card-text class="px-5">
        我是表单
      </v-card-text>
    </v-card>
  </v-dialog>
</template>

<script>
    export default {
        name: "my-brand-form"
    }
</script>

<style scoped>

</style>
