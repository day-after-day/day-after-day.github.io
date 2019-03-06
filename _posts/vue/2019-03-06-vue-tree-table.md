---
layout: post
title: vue树形表格组件封装
categories: vue
description: vue树形表格组件封装
keywords:  vue, table, tree
---


基于element-ui， 以tree组件为主,，简单的封装了一个树形表格组件。
下面是代码

    <template>
      <div class="table-tree-comps"
           ref="tableTree"
           v-loading="tableLoading"
           element-loading-text="加载中"
           element-loading-spinner="el-icon-loading"
           element-loading-background="rgba(255, 255, 255, 0.5)"
           :style="{height: parseInt(height)>=1 ?  parseInt(height)+ 'px' : height}"

      >
        <el-row class="table-header" :style="{'padding-left':checkBoxOffset +'px'}">
          <el-col v-for="(item,idx) in tableKey" :span="item.width" :class="idx===0 ? 'vs': ''" :key="`table-tree-${ idx }`">
            <template v-if="idx===0">
              <div class="full l-move" :style="{position: 'relative', left:-(checkBoxOffset)+'px', width: `calc(100% + ${checkBoxOffset}px)`}">
                {{item.name}}
              </div>
            </template>
            <template v-else>
              {{item.name}}
            </template>
          </el-col>

        </el-row>

        <el-tree
            class="el-tree-table"
            :data="tableData"
            :show-checkbox="showCheckbox"
            :node-key="nodeKey"
            :default-expand-all="defaultExpandAll"
            :expand-on-click-node="expandOnClickNode">
          <el-row class="custom-tree-node" slot-scope="{ node, data }" :style="{width: rowWidth + 'px', 'margin-left': (node.level-1) * (-18)+ 'px'}">
            <el-col v-for="(item,idx) in tableKey" :key="`table-el-tree-${ idx }`" :span="item.width" :style="{'padding-left': idx===0 ? (node.level-1) * 18 + 'px'  : undefined }">
              {{ item.formatter ? item.formatter(data[item.key]) : data[item.key] }}
                <!--<el-button-->
                    <!--type="text"-->
                    <!--size="mini"-->
                    <!--@click="() => append(data,node)">-->
                  <!--Append-->
                <!--</el-button>-->
            </el-col>
          </el-row>
        </el-tree>
      </div>
    </template>

    <script>
      export default {
        name: "tableTree2",
        props: {
          tableData: {type: Array},
          tableKey: { type: Array , require: true}, // 表格字段配置  示例 { key: '字段名' , name: '字段的中文描述' , width: '单元格的宽度（全部为24份）' , formatter: '格式化函数' , operations: { type: '类型' , label: '操作名字' , func:'操作函数'}}
          nodeKey: {type: String, require: true},
          showCheckbox: {type: Boolean, default: false},
          defaultExpandAll:{type: Boolean, default: false},
          expandOnClickNode:{type: Boolean,default: true},
          height: {type: String || Number}
        },
        data(){
          return {
            rowWidth: 0,
            rowMarginLeft: 0,
            tableLoading: false
          }
        },
        computed: {
          checkBoxOffset(){
            let {showCheckbox} = this
            return 24+ (showCheckbox ? 22: 0)
          },
          tableHeight(){
            let {height} = this
            if(parseInt(height)>=1){
              return parseInt(height)+'px'
            }else{
              return height
            }
          }
        },
        mounted(){
          let tableTree =  this.$refs.tableTree;
          this.rowWidth = tableTree.offsetWidth - 2;
          window.onresize =  () => {
            this.rowWidth = tableTree.offsetWidth - 2;
          }
        },
        methods: {
          append(data,node){
            console.log(data)
            console.log(node)
          },
          loading(){
            this.tableLoading = true
          },
          closeLoading(){
            this.tableLoading = false
          }
        },
        filters: {
          formatter(row){

          }
        },
        beforeDestroy(){
          window.onresize = null
        }
      }
    </script>

    <style lang="scss">
      .full{
        width:100%;
        height:100%;
      }
      .vs{
        overflow: visible !important;
      }
      .table-tree-comps{
        border: 1px solid #ebeef5;
        min-height: 450px;
        overflow: hidden;
        .custom-tree-node.el-row, .table-header.el-row {
          width:100%;
          height:46px;
          position: relative;
          z-index: 5;
          .el-col{
            height:100%;
            padding:12px 5px;
            border-right:  1px solid #ebeef5;
            text-align: center;
            line-height:22px;
            @include no-wrap()
          }
        }

        .el-tree-table{
          height:calc(100% - 46px) ;
          overflow: scroll;
          @include scroll-bar(0);
          .el-tree-node__content{
            height: auto;
            border-bottom: 1px solid #ebeef5;
            .el-tree-node__expand-icon{
              position: relative;
              z-index:10;
            }
            .el-checkbox{
              z-index:10;
            }
          }
        }
        .table-header.el-row{
          background-color: #fff;
          border-bottom: 1px solid #ebeef5;
          .el-col{
            border-right:  1px solid #ebeef5;
          }
          .l-move{
            position: relative;
          }
        }
        .cell{
            font-size:12px;
            color:#333c48;
            text-align: center;
          }

      }
    </style>


