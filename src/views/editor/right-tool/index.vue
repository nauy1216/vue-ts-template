<template>
  <div class="right-tool" :style="{ width: width + 'px' }">
    <el-tabs v-model="activeName">
      <el-tab-pane label="布局" name="1">
        <LayoutOperate></LayoutOperate>
      </el-tab-pane>
      <el-tab-pane label="属性" name="2">
        <div v-if="activeCompProps">
          <PropOperate></PropOperate>
        </div>
      </el-tab-pane>
      <!-- <el-tab-pane label="页面" name="3">
        <PageOperate></PageOperate>
      </el-tab-pane>
      <el-tab-pane label="代码" name="4">
        <CodeOperate></CodeOperate>
      </el-tab-pane>
      <el-tab-pane label="编辑器" name="5">
        <EditorOperate></EditorOperate>
      </el-tab-pane> -->
    </el-tabs>
    <div class="right-tool-trigger" ref="trigger"></div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import LayoutOperate from "./layout-operate.vue";
import PropOperate from "./prop-operate.vue";
// import PageOperate from "./page-operate.vue";
// import CodeOperate from "./code-operate.vue";
// import EditorOperate from "./editor-operate.vue";
// import CompTree from "./comp-tree.vue";
import { mapStateTyped } from "@/types/store";

export default Vue.extend({
  data() {
    return {
      activeName: "1",
      width: 250,
      startX: 0,
      startY: 0,
      isDrag: false
    };
  },
  components: {
    LayoutOperate,
    PropOperate
    // PageOperate,
    // CodeOperate,
    // EditorOperate
  },
  computed: {
    ...mapStateTyped("page", ["pageConfig", "activeComp"]),
    ...mapStateTyped("editor", ["editorConfig"]),
    // 当前活动组件的属性
    activeCompProps(): any {
      return this.activeComp && this.$compList[this.activeComp.name];
    }
  },
  created() {
    this.addEvent();
  },
  methods: {
    addEvent() {
      const handleMousemove = (ev: MouseEvent) => {
        if (this.isDrag) {
          this.width += this.startX - ev.clientX;
          this.width = Math.max(200, this.width);
          this.width = Math.min(500, this.width);
          this.startX = ev.clientX;
        }
      };

      const handleMousedown = (ev: MouseEvent) => {
        if ((ev.target as HTMLElement).className === "right-tool-trigger") {
          this.startX = ev.clientX;
          this.startY = ev.clientY;
          this.isDrag = true;
        }
        document.addEventListener("mousemove", handleMousemove);
      };

      const handleMouseup = () => {
        this.isDrag = false;
        document.removeEventListener("mousemove", handleMousemove);
      };

      document.addEventListener("mousedown", handleMousedown);
      document.addEventListener("mouseup", handleMouseup);
    }
  }
});
</script>
<style scoped lang="scss">
.right-tool {
  padding: 20px;
  .right-tool-trigger {
    position: absolute;
    top: 50%;
    left: 0;
    transform: translateY(-50%);
    width: 4px;
    height: 40px;
    background: rgb(64, 158, 255);
    &:hover {
      cursor: ew-resize;
    }
  }
}
</style>
