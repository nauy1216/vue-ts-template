<template>
  <el-form label-position="right" label-width="100px">
    <el-form-item label="页面名称">
      <el-input v-model.number="pageConfig.alias"></el-input>
    </el-form-item>
    <el-form-item label="页面宽度(px)">
      <el-input-number v-model.number="pageConfig.width" :precision="2" :step="1"></el-input-number>
    </el-form-item>
    <el-form-item label="页面高度(px)">
      <el-input-number v-model.number="pageConfig.height" :precision="2" :step="1"></el-input-number>
    </el-form-item>
    <el-form-item label="网格宽度(px)">
      <el-input-number v-model.number="editorConfig.gridX" :precision="0" :step="1"></el-input-number>
    </el-form-item>
    <el-form-item label="网格高度(px)">
      <el-input-number v-model.number="editorConfig.gridY" :precision="0" :step="1"></el-input-number>
    </el-form-item>
    <el-form-item label="缩放">
      <el-input-number v-model.number="editorConfig.zoom" :min="0.1" :max="10" :precision="1" :step="0.1"></el-input-number>
    </el-form-item>
    <!-- <el-form-item label="显示滚动条">
            <el-switch v-model="editorConfig.showScrollbar"> </el-switch>
          </el-form-item> -->
    <el-form-item label="不超出画布">
      <el-switch v-model="editorConfig.parent"> </el-switch>
    </el-form-item>
  </el-form>
</template>

<script lang="ts">
import Vue from "vue";
import { mapMutationsTyped, mapStateTyped } from "@/types/store";
export default Vue.extend({
  computed: {
    ...mapStateTyped("page", ["pageConfig", "activeComp"]),
    ...mapStateTyped("editor", ["editorConfig"])
  },
  created() {
    this.$watch(
      () => {
        return this.pageConfig.width + " " + this.pageConfig.height;
      },
      () => {
        this.refreshPage();
      }
    );
  },
  methods: {
    ...mapMutationsTyped("page", ["refreshPage"])
  }
});
</script>

<style></style>
