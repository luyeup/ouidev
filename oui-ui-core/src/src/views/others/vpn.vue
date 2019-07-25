<template>
<div>
    <br>
    <el-tabs v-model="activeName" type="card">
        <el-tab-pane :label="$t('VPN Status')" name="status">
            <div style="width: 90%;">
                <div class="borderline"></div>
                <div style="float: left;padding-top: 6px;padding-left: 30px;">
                    <span v-text="$t('VPN Name')"></span>
                </div>
                <div style="text-align: right;">
                    <el-select v-model="value" placeholder="select">
                        <el-option
                                v-for="item in options"
                                :key="item.value"
                                :label="item.label"
                                :value="item.value">
                        </el-option>
                    </el-select>
                </div>
                <div class="borderline"></div>
                <br>
                <div style="background-color: white;min-height: 500px;">
                    <pre>{{statusLog}}</pre>
                </div>
                <br>
                <div style="text-align: center">
                    <el-button type="primary" round>{{$t('VPN Abort')}}</el-button>
                </div>
            </div>
        </el-tab-pane>
        <el-tab-pane :label="$t('VPN Management')" name="manager">
            <div  v-for="(item,index) in LCPList"  style="width:50%;margin-left: 100px;margin-top: 30px;">
                <div style="background-color: #bffef2;height: 35px;padding-top: 14px;border-top: 0.5px solid #d3d3d3;border-bottom: 0.5px solid #d3d3d3;">
                    <div style="float: left;margin-left: 10px;">PPTP配置</div><div style="text-align: right;margin-right: 10px;">{{index+1}}</div>
                </div>
                <div style="background-color: #fefed2;height: 34px;padding-top: 13px;">
                    <div style="float: left;margin-left: 10px;">{{item.name}}</div><div style="text-align: right;margin-right: 10px;">
                    <img src="/icons/toUp.png" v-if="item.show==0" @click="item.show=1" width="30px"/>
                    <img src="/icons/toDown.png" v-if="item.show==1" @click="item.show=0" width="30px"/>
                    </div>
                </div>
                <br>
                <el-form :model="LCPForm" :rules="rules" ref="ruleForm" label-width="140px" v-if="item.show==1" class="demo-ruleForm">
                    <el-input v-model="LCPForm.name = item.name" style="display: none;"></el-input>
                    <el-form-item :label="$t('VPN Server')" prop="serverIP">
                        <el-input v-model="LCPForm.serverIP = item.serverIP"></el-input>
                    </el-form-item>
                    <el-form-item :label="$t('VPN Lcp-echo-failure')" prop="threshold">
                        <el-input v-model="LCPForm.threshold = item.threshold"></el-input>
                    </el-form-item>
                    <el-form-item :label="$t('VPN Lcp-echo-interval')" prop="interval">
                        <el-input v-model="LCPForm.interval = item.interval"></el-input>
                    </el-form-item>
                    <el-form-item :label="$t('VPN User Name')" prop="userName">
                        <el-input v-model="LCPForm.userName = item.userName"></el-input>
                    </el-form-item>
                    <el-form-item :label="$t('VPN Password')" prop="password">
                        <el-input v-model="LCPForm.password = item.password"></el-input>
                    </el-form-item>
                    <el-form-item style="text-align: center">
                        <el-button  @click="removeConfig('LCPForm')">{{$t('VPN Delete')}}</el-button>
                        <el-button type="primary"  @click="sumitConfig('LCPForm')">{{$t('VPN Apply')}}</el-button>
                    </el-form-item>
                </el-form>
            </div>
            <div class="borderline"></div>
            <div style="width:50%;margin-top: 15px;text-align: right;">
                <el-button type="primary" style="width: 78%;" round @click="centerDialogVisible = true">+{{$t('Add VPN')}}</el-button>
            </div>
            <el-dialog
                    style=""
                    :title="$t('Add VPN')"
                    :visible.sync="centerDialogVisible"
                    width="50%"
                    center
                    >
                <div  style="text-align: center">
                    <el-form :model="addForm" :rules="rules" ref="ruleForm" label-width="140px" class="demo-ruleForm">
                        <el-form-item :label="$t('VPN Name')" prop="addFormName">
                            <el-input v-model="addForm.name" ></el-input>
                        </el-form-item>
                        <el-form-item :label="$t('VPN Server')" prop="addFormServerIP">
                            <el-input v-model="addForm.serverIP"></el-input>
                        </el-form-item>
                        <el-form-item :label="$t('VPN Lcp-echo-failure')" prop="addFormThreshold">
                            <el-input v-model="addForm.threshold"></el-input>
                        </el-form-item>
                        <el-form-item :label="$t('VPN Lcp-echo-interval')" prop="addFormInterval">
                            <el-input v-model="addForm.interval"></el-input>
                        </el-form-item>
                        <el-form-item :label="$t('VPN User Name')" prop="addFormUserName">
                            <el-input v-model="addForm.userName"></el-input>
                        </el-form-item>
                        <el-form-item :label="$t('VPN Password')" prop="addFormPassword">
                            <el-input v-model="addForm.password"></el-input>
                        </el-form-item>
                    </el-form>
                    <span slot="footer" class="dialog-footer">
                        <el-button @click="centerDialogVisible = false">{{$t('VPN Cancel')}}</el-button>
                        <el-button type="primary"  @click="sumitConfig('addForm')" >{{$t('VPN Add')}}</el-button>
                    </span>
                </div>
            </el-dialog>



        </el-tab-pane>
    </el-tabs>

</div>

</template>
<script>
    import ElButton from "../../../node_modules/element-ui/packages/button/src/button.vue";
    import ElInput from "../../../node_modules/element-ui/packages/input/src/input.vue";

    export default {
        components: {
            ElInput,
            ElButton},
        data() {
            return {
                LCPList: [],
                centerDialogVisible: false,
                activeName : 'status',
                LCPForm: {
                    name: '',
                    serverIP: '',
                    threshold: '',
                    interval: '',
                    userName: '',
                    password: ''
                },
                addForm: {
                    name:'',
                    serverIP: '',
                    threshold: '',
                    interval: '',
                    userName: '',
                    password: ''
                },
                rules: {
                    serverIP:[
                        {required:true}
                    ],
                    threshold:[
                        {required:true}
                    ],
                    interval:[
                        {required:true}
                    ],
                    userName:[
                        {required:true}
                    ],
                    password:[
                        {required:true}
                    ]
//                    addFormName:[
//                        {required:true}
//                    ],
//                    addFormServerIP:[
//                        {required:true}
//                    ],
//                    addFormThreshold:[
//                        {required:true}
//                    ],
//                    addFormInterval:[
//                        {required:true}
//                    ],
//                    addFormUserName:[
//                        {required:true}
//                    ],
//                    addFormPassword:[
//                        {required:true}
//                    ]
//                    name: [
//                        { required: true, message: '请输入活动名称', trigger: 'blur' },
//                        { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
//                    ],
//                    region: [
//                        { required: true, message: '请选择活动区域', trigger: 'change' }
//                    ],
//                    date1: [
//                        { type: 'date', required: true, message: '请选择日期', trigger: 'change' }
//                    ],
//                    date2: [
//                        { type: 'date', required: true, message: '请选择时间', trigger: 'change' }
//                    ],
//                    type: [
//                        { type: 'array', required: true, message: '请至少选择一个活动性质', trigger: 'change' }
//                    ],
//                    resource: [
//                        { required: true, message: '请选择活动资源', trigger: 'change' }
//                    ],
//                    desc: [
//                        { required: true, message: '请填写活动形式', trigger: 'blur' }
//                    ]
                },
                options: [{
                    value: '1',
                    label: 'shumi01'
                }, {
                    value: '2',
                    label: 'shumi02'
                }],
                value: '',
                statusLog:''   //改变 这个值 既可以改变状态中的日志信息

            };
        },
        created(){ //页面加载时执行

        },
        mounted(){  //页面加载完成后执行

        },
        methods: {
            sumitConfig(formName) {
                debugger
                if ("addForm" == formName){
                            var addform = {};
                            addform.name = this.addForm.name;
                            addform.show = 0;
                            addform.serverIP = this.addForm.serverIP;
                            addform.threshold = this.addForm.threshold;
                            addform.interval = this.addForm.interval;
                            addform.userName = this.addForm.userName;
                            addform.password = this.addForm.password;
                            this.LCPList.push(addform);
                            console.log("----------------");
                            console.log(this.LCPList);
                            console.log("----------------");
                            this.centerDialogVisible = false
                }
//                this.$refs[formName].validate((valid) => {
//                    if (valid) {
//                        if ("addForm" == formName){
//                            var addform = {};
//                            addform.name = this.addForm.name;
//                            addform.serverIP = this.addForm.serverIP;
//                            addform.threshold = this.addForm.threshold;
//                            addform.interval = this.addForm.interval;
//                            addform.userName = this.addForm.userName;
//                            addform.password = this.addForm.password;
//                            this.LCPList.push(addform);
//                        }
//                        alert('submit!');
//                    } else {
//                        console.log('error submit!!');
//                        return false;
//                    }
//                });
            },
            removeConfig(formName) {
                this.$refs[formName].resetFields();
            }
        }
    }
</script>
<style>
    .borderline{
        border-top: 0.5px solid #d3d3d3;
        padding-top: 10px;
        margin-top: 10px;
    }
</style>


