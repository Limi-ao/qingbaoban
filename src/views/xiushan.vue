<template>
    <div>
        <iframe :src="url" frameborder="0" width="100%" style="height:calc(100vh - 64px)"></iframe>
        <el-dialog title="微波检测器" :visible.sync="wbjcyVisible" width="540px">
            <el-form :model="wbjcyForm"  label-width="100px">
                <el-form-item label="设备名称">
                    <el-input v-model="wbjcyForm.name" style="width:300px"></el-input>
                </el-form-item>
                <el-form-item label="设备信息">
                    <el-card style="width:300px">
                        暂无内容
                    </el-card>
                </el-form-item>
            </el-form>
            <div class="center">
                <el-button type="primary" @click="editDevice('wbjcyForm')">确认修改</el-button>
            </div>
        </el-dialog>
        <el-dialog title="气象仪" :visible.sync="qxyVisible" width="540px">
            <el-form :model="qxyForm"  label-width="100px">
                <el-form-item label="设备名称">
                    <el-input v-model="qxyForm.name" style="width:300px"></el-input>
                </el-form-item>
                <el-form-item label="设备名称">
                    <el-card style="width:300px;">

                    </el-card>
                </el-form-item>
            </el-form>
            <div class="center">
                <el-button type="primary" @click="editDevice('qxyForm')">确认修改</el-button>
            </div>
        </el-dialog>
        <el-dialog title="门架式情报板" :visible.sync="mjsqbbVisible" width="540px">
            <el-form :model="mjsqbbForm"  label-width="100px">
                <el-form-item label="设备名称">
                    <el-input v-model="mjsqbbForm.name" style="width:300px"></el-input>
                </el-form-item>
                <el-form-item label="显示文字">
                    <el-input type="textarea" v-model="mjsqbbForm.info" resize="none" placeholder="设备显示信息" style="width:300px"></el-input>
                </el-form-item>
            </el-form>
            <div class="center">
                <el-button type="primary" @click="editDevice('mjsqbbForm')">确认修改</el-button>
            </div>
        </el-dialog>
    </div>
</template>

<script>
    export default {
        name:'Xiushan',
        data(){
            return {
                url:'/static/pages/xsdq.html',
                mjsqbbVisible:false,
                qxyVisible:false,
                wbjcyVisible:false,
                mjsqbbForm:{
                    name:"",
                    info:""
                },
                qxyForm:{
                    name:"",
                    info:""
                },
                wbjcyForm:{
                    name:"",
                    info:""
                }
            }
        },
        mounted(){
            window.selectDevice = this.selectDevice
        },
        methods:{
            selectDevice(name){
                if(name.indexOf("GDSXJ") !== -1){
                   this.$router.push({path:'/video',query:{deviceName:name}});
                }else if(name.indexOf('YKSXJ') !== -1){
                    this.$router.push({path:'/video',query:{deviceName:name}});
                }else if(name.indexOf('QXG') !== -1){
                    this.qxyForm.name = name;
                    let _this = this
                    setTimeout(function() {
                        _this.qxyVisible = true
                    }, 200)
                }else if(name.indexOf('WBJCY') !== -1){
                    this.wbjcyForm.name = name;
                    let _this =  this;
                    setTimeout(function(){
                        _this.wbjcyVisible = true
                    },200)
                }else if(name.indexOf('MJSQBB') !== -1){
                    this.mjsqbbForm.name = name;
                    let _this = this;
                    setTimeout(function(){
                        _this.mjsqbbVisible = true
                    },200)
                }
            }
        }
    }
</script>

<style scoped>
.center{
    text-align: center
}
</style>