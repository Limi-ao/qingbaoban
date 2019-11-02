<template>
    <div class="qbb-content">
       <div class="qbb-tool">
           <ul>
               <li><i class="el-icon-edit"></i><a @click="handlefs">信息发送</a></li>
               <li><i class="el-icon-edit"></i><a @click="dialomenage">信息管理</a></li>
               <li><i class="el-icon-edit"></i><a>开启群发</a></li>
               <li><i class="el-icon-edit"></i><a>关闭群发</a></li>
           </ul>
       </div>
        <!-- 信息管理弹出框 -->
        <el-dialog title="信息管理" :close-on-click-modal="false" :visible.sync="dialogVisible" >
             <manage></manage>
        </el-dialog>

       <div class="qbb-contianer">
          <el-row :gutter="10">
            <el-col :span="4">
                <div class="grid-content bg-purple">
                    <div class="grid-top">相同种类设备列表</div>
                    <div style="height:300px; background:#fff;">
                        <el-tree
                            
                            :data="data"
                            show-checkbox
                            node-key="id"
                            default-expand-all
                            :default-checked-keys="[5]"
                            :props="defaultProps" >
                            
                        </el-tree>
                    </div>
                    <div style="background:#fff; height:415px;"></div>
                </div>
            </el-col>

  
            <!--发送界面-->
            <el-col :span="19">
                <div class="grid-content bg-purple">
                    <el-tabs v-model="activeName" @tab-click="handleClick">
                        <el-tab-pane label="发送界面" name="first">
                            <!--左边-->
                            <div class="grid-fsleft">
                                <el-form :model="ruleForm" ref="ruleForm" label-width="100px" class="demo-ruleForm" size="mini">
                                    <el-form-item label="信息查询:" prop="name" class="item item-fsleft">
                                        <el-input v-model="ruleForm.name" style="width:178px;"><el-button slot="append" icon="el-icon-search"></el-button></el-input>
                                    </el-form-item>
                                
                                    <el-form-item label="信息类型:" class="item item-fsleft">
                                        <el-select v-model="value" placeholder="-请选择-" @change="currentSel()">
                                            <el-option
                                                v-for="item in options"
                                                :key="item.value"
                                                :label="item.content"
                                                :value="item.typeId">
                                            </el-option>
                                        </el-select>
                                    </el-form-item>
                                </el-form>
                                <div class="grid-fsbottom">
                                    <ul class="grid-top" style=" height:595px;">
                                        <li style=" list-style-type:none; border-bottom:1px solid #BFBDBD;padding-left:10px;color:#4D4E52;" @dblclick="tianbiao" v-for="itli in disli" :key="itli.infoId" :ke="itli.infoId">{{itli.display}}</li>
                                    </ul>
                                   <!--  <div style="height:555px;">
                                       {{value}}
                                    </div> -->
                                </div>
                            </div>
                            <!--右边-->
                            <div class="grid-fsright">
                                <!--内容显示-->
                                <el-row>
                                    <el-col :span="23"><div class="grid-content bg-purple-dark">
                                            <div class="grid-top">内容显示</div>
                                            <div style="height:120px;"><div class="grid-fsright-qbb">dsfdsfdf</div> </div>
                                            </div>
                                    </el-col>
                                </el-row>
                                <!--发送列表-->
                                <el-row>
                                    <el-col :span="23"><div class="grid-content bg-purple-dark">
                                            <div class="grid-topo">发送列表
                                                <span style="margin-left:850px; color:#09f;">
                                                    <i class="el-icon-top-left" style="cursor: pointer; margin-right:15px;">移出队列</i>
                                                    <i class="el-icon-close" style="cursor: pointer;">清空队列</i>
                                                </span>
                                            </div>
                                            <div style="height:200px;"> 
                                                <el-table :data="tableData" border :header-cell-style="tableHeaderColor"  highlight-current-row style="width:100%; border:1px solid #DDDDDD;">
                                                        <el-table-column type="selection" width="55">
                                                        </el-table-column>
                                                        <el-table-column prop="infoId" label="信息编号" width="100">
                                                        </el-table-column>
                                                        <el-table-column prop="display" label="发布内容" width="180">
                                                        </el-table-column>
                                                        <el-table-column prop="fontType" label="字体类型" width="100">
                                                        </el-table-column>
                                                        <el-table-column prop="fontColor" label="字体颜色" width="100">
                                                        </el-table-column>
                                                        <el-table-column prop="fontSize" label="字体大小" width="100">
                                                        </el-table-column>
                                                        <el-table-column prop="imageCode" label="发布图片" min-width="80" >
                                                        </el-table-column>
                                                </el-table>
                                            </div>
                                            </div>
                                    </el-col>
                                </el-row>

                                <el-row>
                                    <!--参数-->
                                    <el-col :span="7"><div class="grid-content bg-purple-dark">
                                            <div class="grid-topo">参数</div>
                                            <div style="height:226px;"> 
                                                <el-form ref="form" :model="sizeForm" label-width="100px" size="mini">
                                                        <el-form-item label="显示方式:" class="item" style="margin-top:4px; margin-bottom:4px; width:277px;">
                                                            <el-input v-model="sizeForm.xianshi" placeholder="立即显示"></el-input>
                                                        </el-form-item>
                                                        <el-form-item label="动作速度:" class="item" style="margin-bottom:4px;">
                                                            <el-select v-model="sizeForm.sudu" placeholder="0">
                                                                <el-option label="0" value="0"></el-option>
                                                                <el-option label="1" value="1"></el-option>
                                                            </el-select>
                                                        </el-form-item>
                                                        <span style="color:red;margin-left:36px;">(注:选择范围0~9,其中0表示最快)</span>
                                                         <el-form-item label="停留时间(秒):" class="item" style="margin-top:4px; margin-bottom:4px;">
                                                            <el-select v-model="sizeForm.shijian" placeholder="5">
                                                                <el-option label="2" value="2"></el-option>
                                                                <el-option label="3" value="3"></el-option>
                                                            </el-select>
                                                        </el-form-item>
                                                         <el-form-item label="发布人员:" class="item" style="margin-top:4px; margin-bottom:4px; width:225px;">
                                                            <el-input v-model="sizeForm.renyuan" ></el-input> 
                                                        </el-form-item>
                                                            <div style="float:right;margin-top:-30px;margin-right:60px;color:#09f;cursor: pointer;"><i class="el-icon-edit-outline">修改</i></div>
                                                         <el-form-item label="信息来源:" class="item">
                                                            <el-select v-model="sizeForm.xinxi" placeholder="监控中心">
                                                                <el-option label="监控中心" value="jiankongzhongxin"></el-option>
                                                                <el-option label="中心下发" value="zhongxinxiafa"></el-option>
                                                                <el-option label="交警代发" value="jiaojindaifa"></el-option>
                                                            </el-select>
                                                        </el-form-item>
                                                        <el-form-item style="margin-top:-10px; ">
                                                                <i class="el-icon-plus" style="color:#09f;margin-right:6px; cursor: pointer;" @click="handleaddt">添加</i>
                                                                <i class="el-icon-document-add" style="color:#09f; margin-left:6px; cursor: pointer;" @click="handleaddp" >批次加</i>
                                                        </el-form-item>
                                                </el-form>
                                            </div>
                                       </div>
                                    </el-col>
                                    <!--发送结果-->
                                    <el-col :span="16.5"><div class="grid-content bg-purple-dark sj-grid">
                                            <div class="grid-topo">发送结果</div>
                                            <div style="height:230px;"> 
                                                <el-table :data="tableData" border :header-cell-style="tableH"  highlight-current-row  style="width:100%; border:1px solid #DDDDDD;">
                                                        <el-table-column prop="name" label="设备名称" width="254">
                                                        </el-table-column>
                                                        <el-table-column prop="sort" label="发布结果" width="255">
                                                        </el-table-column>
                                                        <el-table-column prop="zt" label="设备状态"  width="255">
                                                        </el-table-column>
                                                </el-table>
                                            </div>
                                        </div>
                                    </el-col>
                                </el-row>
                            </div>
                        </el-tab-pane>

                        <!--编辑信息界面-->
                        <el-tab-pane label="编辑信息界面" name="second"><bian-ji></bian-ji></el-tab-pane>
                    </el-tabs>
                </div>
            </el-col>
        </el-row>
       </div>
    </div>
</template>

<script>
import manage from './manage.vue'
import BianJi from './bianji.vue'
import axios from 'axios'
    export default {
       components:{manage,BianJi},
        data(){
            return{
                dialogVisible: false,//信息管理
                activeName: 'first',
                ruleForm: {
                    name:'',
                },
                sizeForm: {
                    xianshi: '',
                    sudu: '',
                    shijian:'',
                    renyuan:'1111',
                    xinxi:''
                },
                // filters:{//弹出框查询
                //     shebei:'',
                //     yangshi:''
                // },
                newItem:[],
                 tableData:[{
                     infoId:'31',
                     display:'',
                     fontSize:'xiao'
                     }
                 ],
                

                 data: [{
                    id: 1,
                    label: '关山大桥',
                    children: [{
                        id: 4,
                        label: 'k1+1000',
                    },{
                        id: 9,
                        label: 'k1+2000',
                    },
                    {
                        id: 10,
                        label: 'k1+3000',
                    },
                    ]
                    }, {
                    id: 2,
                    label: '炮台山隧道',
                    children: [{
                        id: 5,
                        label: 'k2+1000'
                    }, {
                        id: 6,
                        label: 'k2+2000'
                    }]
                    }, {
                    id: 3,
                    label: '秀山大桥',
                    children: [{
                        id: 7,
                        label: 'k3+1000'
                    }, {
                        id: 8,
                        label: 'k3+2000'
                    }]
                    }],
                    defaultProps: {
                    children: 'children',
                    label: 'label'
                    },
                    options: [//选择
                        ],
                    value: '',
                    disli:[]
               // selVal : ''
                    }
                },
        methods:{
            //进入页面 获取左侧
            handlejinru(){
                
            },
            //查询类型显示
            handleleixin(){
                   axios.get('http://127.0.0.1:8080/static/mock/data.json').then(res=>{
                    console.log('leixin',res.data.leixinxianshi)
                    this.options=res.data.leixinxianshi
                })
            },
            handleClick(){},
            tableHeaderColor({ row, column, rowIndex, columnIndex }) {
                if (rowIndex === 0) {
                    return 'background-color: #F9F9F9;font-weight:600;font-size:14px;text-align:center; color:#000;'
            }
            },
          tableH({ row, column, rowIndex, columnIndex }){
             if (rowIndex === 0) {
                    return 'background-color: #DAE6F4;font-weight:600;font-size:14px;text-align:center; color:#000;'
            }
           },
           //弹出框
          dialomenage(){
            this.dialogVisible = true;
          },
        //信息查询
        handleso(){},
        //信息类型查询
        currentSel(value){
            console.log(value)
            axios.get('http://127.0.0.1:8080/static/mock/data.json').then(res=>{
                console.log(res.data.xinxileixin)
                this.disli=res.data.xinxileixin
                
            })
        }, 
           //双击事件
           tianbiao(disli){
                console.log("k",this.disli)
            //    console.log("dfd")
            //  let newItem=[{
            //        name:'dfd',
            //        sort:'d',
            //        zt:'fdfd'
            //   }]
            //   this.$set(this.tableData,this.tableData.length,newItem)
           },      
           //添加
           handleaddt(){},
           //批次添加  
           handleaddp(){},
           //信息发送
           handlefs(){},
        },
        mounted(){
           this.handlejinru();
           this.handleleixin();
        },
    }
</script>

<style scoped>

 .qbb-content{
      color: initial;
      height: 850px;
      border: 1px solid #DDDDDD;
      margin: auto 15px;
   }
   .qbb-tool{
       height: 60px;
       border: 1px solid #DDDDDD;
       box-shadow: 0 0 25px #cac6c6;
       background: rgb(255, 255, 255);
       margin-bottom:20px;
   }
   .qbb-contianer{
       background: #fff;
       height: 780px;
   }
     .qbb-tool ul li{
         float: left;
         margin-left:30px;
         line-height: 60px;
     }
       .qbb-tool ul li i{
          color: #09f;
          margin-right:10px;
       }
       .qbb-tool ul li a{
           cursor: pointer;
           color: #565A5D;
       }
  
  .grid-top{
      line-height: 40px;
      padding-left:10px;
      height: 40px;
      border-bottom: 1px solid #828181;
  }
    .grid-topo{
      line-height: 30px;
      padding-left: 10px;
      height: 30px;
      border-bottom: 1px solid #828181;
  }
  .el-row {
    margin-bottom: 20px;
  }
  .el-col {
    border-radius: 4px;
    margin-top: 20px;
    margin-left: 25px;
  }
  .bg-purple-dark {
    background: #fff;
  }
  .bg-purple {
    background: #F6F6F6;
  }
  .bg-purple-light {
    background: #e5e9f2;
  }
  .grid-content {
    border-radius: 4px;
    min-height: 36px;
    border: 1px solid #BFBDBD;
  }
  .row-bg {
    padding: 10px 0;
    background-color: #f9fafc;
  }
  .grid-fsleft{
      float: left;
      width: 300px;
      height: 680px;
     /* border: 1px solid #000; */
  }
 .grid-fsright{
    height: 700px;
     /* border: 1px solid #000; */
     margin-left: 310px;
     
    
 }
 .grid-fsbottom{
     /* border: 1px solid #000; */
     border: 1px solid #BFBDBD;
     margin-left: 10px;
     margin-right: 10px;
     border-radius: 4px;
 }
 .grid-fsright .el-row{
     height: 100px;
 }
 .grid-fsright .el-col{
     margin-left: 10px;
     margin-top: 0px;
     margin-bottom: 8px;
    
 }
 .sj-grid{
     width: 768px;
 }
.item-fsleft{
    margin-left:-20px;
}
.grid-fsright-qbb{
   background:#000;
   width:100px;
   height:100px;
   color:#fff;
   line-height: 100px;
   text-align: center;
   margin-left: 500px;
   margin-top: 20px;
}
</style>

<style>
.qbb-content .item .el-form-item__label {
    color:#000;
    font-size: 12px;
}
.qbb-content .el-table__header tr,
  .el-table__header th {
    padding: 0;
    height: 35px;
    border-color: #CFD1D3; 
}
.qbb-content .el-table__body tr,
  .el-table__body td {
    padding: 0;
    height: 35px;
    border-color: #CFD1D3; 
}
.qbb-content .el-table td, .el-table th.is-leaf,.el-table--border, .el-table--group{
  border-color: #CFD1D3; 
}
.sj-grid .el-table__header tr{
    
    background: #09f;
}
/* 图表内容居中 */
.el-table .cell, .el-table th div, .el-table--border td:first-child .cell, .el-table--border th:first-child .cell {
  
    text-align: center;
}
.qbb-content .el-dialog__wrapper .el-dialog__header{
  /* text-align: center;
  padding: 0px 20px 10px; */
  background: #3398CC;
  text-align: left;
  height: 50px;
  line-height: 50px;
}
.qbb-content .el-dialog__close.el-icon.el-icon-close{
    margin-top:-20px;
    padding-top: -10px;
}
.qbb-content .el-dialog__body{
      margin-left:30px;
      margin-right: 30px;}

 .qbb-content .el-dialog__wrapper .el-dialog{
  color: #ffffff;
  background: #fff;
  background-size: 100% 100%  ;
}
</style>