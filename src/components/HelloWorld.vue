<template>
  <div>
    <button @click="isShowUserConfigModel = true">
      Open Modal
    </button>
    <NModal
      v-model:show="isShowUserConfigModel"
      preset="card"
      size="huge"
      title="用户配置"
      transform-origin="mouse"
      width="1300px"
    >
      <vxe-grid v-bind="gridOptions"></vxe-grid>
    </NModal>
  </div>
</template>

<script lang="ts" setup>
import { reactive, ref } from "vue"
import type {
  VxeGridProps,
  VxeColumnPropTypes,
} from "vxe-table"
import type { VxeSelectProps } from "vxe-pc-ui"
import { data } from "./data"

const isShowUserConfigModel = ref(true)

interface RowVO {
  id: number
  name: string
  role: string
  sex: string
  sexList: string[]
  nationality: string
  type: string
  typeList: string[]
}

const typeEditRender = reactive<
  VxeColumnPropTypes.EditRender<RowVO, VxeSelectProps>
>({
  name: "VxeSelect",
  props: {
    // 弹窗遮挡了面板，因此在 src/main.ts 中设置了 zIndex: 3000
    // 此时发现弹窗中可编辑表格的 VxeSelect 组件无法进行搜索
    filterable: true,
    clearable: true,
  },
  optionGroups: [
    {
      label: "分类1",
      options: [
        { label: "苹果", value: "1-1" },
        { label: "雪梨", value: "1-2" },
      ],
    },
    {
      label: "分类2",
      options: [
        { label: "草莓", value: "2-1" },
        { label: "猕猴桃", value: "2-2" },
      ],
    },
  ],
})

const roleEditRender = reactive<
  VxeColumnPropTypes.EditRender<RowVO, VxeSelectProps>
>({
  name: "VxeSelect",
  props: {
    filterable: true,
  },
  options: [],
})

const gridOptions = reactive<VxeGridProps<RowVO>>({
  border: true,
  editConfig: {
    trigger: "click",
    mode: "row",
  },
  columns: [
    { type: "seq", width: 70 },
    {
      field: "type",
      title: "下拉分组单选",
      width: 200,
      editRender: typeEditRender,
    },
    {
      align: "center",
      editRender: {
        name: "VxeSelect",
        optionGroupProps: {
          options: "children",
        },
        optionGroups: data,
        props: { clearable: true, filterable: true },
      },
      field: "nationality",
      showOverflow: "tooltip",
      // slots: { edit: 'nationality-edit' },
      title: "国籍",
      width: 200,
    },
    {
      field: "role",
      title: "大数据量选项",
      width: 200,
      editRender: roleEditRender,
    },
  ],
  data: [
    {
      id: 10001,
      name: "Test1",
      role: "role2",
      sex: "",
      sexList: [],
      type: "",
      nationality: "",
      typeList: [],
    },
    {
      id: 10002,
      name: "Test2",
      role: "role10",
      sex: "Women",
      sexList: ["Man", "Women"],
      type: "2-1",
      nationality: "",
      typeList: ["1-2", "2-1"],
    },
    {
      id: 10003,
      name: "Test3",
      role: "role200",
      sex: "Man",
      sexList: [],
      type: "",
      nationality: "",
      typeList: [],
    },
  ],
})

// 模拟后端接口
setTimeout(() => {
  const list: any[] = []
  for (let i = 0; i < 10000; i++) {
    list.push({
      value: `role${i}`,
      label: `角色${i}`,
    })
  }
  roleEditRender.options = list
}, 100)
</script>
