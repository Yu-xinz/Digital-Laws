<template>
  <div class="lower-right">
    <div class="list-container">
      <ul>
        <li v-for="item in items" :key="item.id" @click="showFullText(item)">
          {{ item.name }}
        </li>
      </ul>
    </div>
    <v-dialog
      v-model="dialog"
      scrollable
      max-width="800px"
    >
      <v-card>
        <v-card-title>
          {{ selectedItem.name }}
        </v-card-title>
        <v-divider></v-divider>
        <v-card-text>
          {{ selectedItem.fullText }}
        </v-card-text>
      </v-card>
    </v-dialog>
  </div>
</template>

<style scoped>

.modal-content {
  max-height: 80vh; /* 限制模态框的最大高度为 80% 视窗高度 */
  overflow-y: auto; /* 允许内容溢出时滚动 */
  padding: 20px;
  background-color: white;
  border-radius: 5px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  text-align: left;
  position: relative;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%); /* 居中显示 */
}

.lower-right {
  flex: 1; /* 占据 1/4 的宽度 */
  width: 400px;
  background-color: #faf8f8;
  height: 100%;
  margin-left: 20px;
  box-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
  overflow-y: auto;
}

.list-container {
  height: calc(100% - 20px);
  overflow-y: auto;
}

ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

li {
  padding: 10px;
  border-bottom: 1px solid #ddd;
  transition: box-shadow 0.2s; /* 添加过渡效果 */
}

li:last-child {
  border-bottom: none;
}

li:hover {
  background-color: rgba(224, 224, 224, 0.5); /* 淡化的背景颜色 */
  box-shadow: 4px 4px 4px rgba(0, 0, 0, 0.3); /* 添加阴影效果 */
}
</style>

<script lang="ts">
import jsonData from "@/assets/output.json"

export default {
  data() {
    return {
      items: [],
      dialog: false,
      selectedItem: null,
    };
  },
  mounted() {
    this.processJsonData();
  },
  methods: {
    processJsonData() {
      this.items = jsonData.map(item => ({
        id: item.id,
        name: item["案件名称"],
        fullText: item["全文"],
      }));
    },
    showFullText(item) {
      this.selectedItem = item;
      this.dialog = true
    },
  },
};
</script>