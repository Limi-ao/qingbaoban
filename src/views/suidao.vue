<template>
    <div>
        <iframe :src="url" frameborder="0" width="100%" style="height:calc(100vh - 64px)"></iframe>
        <el-dialog title="车道指示灯" :visible.sync="cdzsdVisible" width="500px">
            <el-form :model="cdzsdForm"  label-width="120px">
            <el-form-item label="设备名称">
                <el-input v-model="cdzsdForm.name" style="width:210px"></el-input>
            </el-form-item>
            <el-form-item label="设备状态">
                <el-radio-group v-model="cdzsdForm.state">
                    <el-radio-button label="2">通行</el-radio-button>
                    <el-radio-button label="3">禁止通行</el-radio-button>
                    <el-radio-button label="1">在线</el-radio-button>
                    <el-radio-button label="0">离线</el-radio-button>
                    <el-radio-button label="-1">故障</el-radio-button>
                </el-radio-group>
            </el-form-item>
            </el-form>
            <div class="center">
                <el-button type="primary" @click="editDevice('cdzsdForm')">确认修改</el-button>
            </div>
        </el-dialog>
        <el-dialog title="四可变信号灯" :visible.sync="skbxhdVisible" width="500px">
            <el-form :model="skbxhdForm"  label-width="150px" >
                <el-form-item label="设备名称">
                    <el-input v-model="skbxhdForm.name" style="width:210px"></el-input>
                </el-form-item>
                <el-form-item label="设备左行">
                    <el-radio-group v-model="skbxhdForm.leftState">
                        <el-radio-button label="1">左转</el-radio-button>
                        <el-radio-button label="0">禁止左转</el-radio-button>
                    </el-radio-group>
                </el-form-item>
                <el-form-item label="设备信号灯">
                    <el-radio-group v-model="skbxhdForm.state">
                        <el-radio-button label="1">绿灯</el-radio-button>
                        <el-radio-button label="2">黄灯</el-radio-button>
                        <el-radio-button label="3">红灯</el-radio-button>
                    </el-radio-group>
                </el-form-item>
            </el-form>
            <div class="center"><el-button type="primary" @click="editDevice('skbxhdForm')">确认修改</el-button></div>
            
        </el-dialog>
        <el-dialog title="悬臂式情报板" :visible.sync="xbsqbbVisible" width="540px">
            <el-form :model="xbsqbbForm"  label-width="120px">
                <el-form-item label="设备名称">
                    <el-input v-model="xbsqbbForm.name" placeholder="设备名称" style="width:300px"></el-input>
                </el-form-item>
                <el-form-item label="显示文字">
                    <el-input type="textarea" v-model="xbsqbbForm.info" resize="none" placeholder="设备显示信息" style="width:300px"></el-input>
                </el-form-item>
            </el-form>
            <div class="center">
                <el-button type="primary" @click="editDevice('xbsqbbForm')">确认修改</el-button>
            </div>
        </el-dialog>
    </div>
</template>

<script>
import baseAlert from '@/components/alert'
    export default {
        name:'Suidao',
        components:{baseAlert},
        data(){
            return {
                url:'/static/pages/shuidao.html',
                cdzsdVisible:false,
                skbxhdVisible:false,
                xbsqbbVisible:false,
                cameraName:"",
                xbsqbbForm:{
                    name:"",
                    info:""
                },
                cdzsdForm:{
                    name:"",
                    state:""
                },
                skbxhdForm:{
                    name:"",
                    state:"",
                    leftState:""
                }
            }
        },
        mounted(){
            window.selectDevice = this.selectDevice; 
        },
        methods:{
            selectDevice(name){
                let _this = this
                if(name.indexOf("CDZSD") !== -1){
                    this.cdzsdForm.name = name;
                    setTimeout(function() {
                        _this.cdzsdVisible = true
                    }, 200)
                }else if(name.indexOf('SKBXHD') !== -1){
                    this.skbxhdForm.name = name;
                    setTimeout(function() {
                        _this.skbxhdVisible = true
                    }, 200)
                }else if(name.indexOf('GDSXJ') !== -1){
                    this.$router.push({path:'/video',query:{deviceName:name}})
                }else if(name.indexOf('XBSQBB') !== -1){
                    this.xbsqbbForm.name = name;
                    setTimeout(function(){
                        _this.xbsqbbVisible = true
                    })
                }
            },
            editDevice(name){
                switch(name){
                    case 'cdzsdForm':
                        this.$message({
                            type:'success',
                            message:'修改成功'
                        });
                        this.cdzsdVisible = false
                        window.Object3D.modelBussiness.changeDevState(this.cdzsdForm.name,this.cdzsdForm.state);
                        break;
                    case 'skbxhdForm':
                        this.$message({
                            type:'success',
                            message:'修改成功'
                        });
                        let state="1";
                        state += this.skbxhdForm.leftState;
                        if(this.skbxhdForm.state === "1"){
                            state += "100"
                        }else if(this.skbxhdForm.state === "2"){
                            state += "010";
                        }else{
                            state += "001"
                        }
                        this.skbxhdVisible = false
                        window.Object3D.modelBussiness.changeDevState(this.skbxhdForm.name,state);
                        break;
                    case 'xbsqbbForm':
                        break;
                    default:
                        
                }
            }
        },
    }
</script>

<style scoped>
.center{
    text-align: center
}
</style>