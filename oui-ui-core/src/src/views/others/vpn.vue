<template>
<div>
    <br>
    <el-tabs v-model="activeName" type="card">
        <el-tab-pane :label="$t('VPN Status')" name="status">
            <div style="width: 90%;">
                <hr>
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
                <hr>
                <br>
                <div style="background-color: white;min-height: 500px;">
                    <pre>{{statusLog}}</pre>
                </div>
                <div style="text-align: center">
                    <el-button type="primary" round>{{$t('VPN Abort')}}</el-button>
                </div>
            </div>
        </el-tab-pane>
        <el-tab-pane :label="$t('VPN Management')" name="manager">
            <div style="width:50%;margin-left: 100px;margin-top: 30px;">
                <el-form :model="LCPForm" :rules="rules" ref="ruleForm" label-width="140px" class="demo-ruleForm">
                    <el-form-item :label="$t('VPN Server')" prop="serverIP">
                        <el-input v-model="LCPForm.serverIP"></el-input>
                    </el-form-item>
                    <el-form-item :label="$t('VPN Lcp-echo-failure')" prop="threshold">
                        <el-input v-model="LCPForm.threshold"></el-input>
                    </el-form-item>
                    <el-form-item :label="$t('VPN Lcp-echo-interval')" prop="interval">
                        <el-input v-model="LCPForm.interval"></el-input>
                    </el-form-item>
                    <el-form-item :label="$t('VPN User Name')" prop="userName">
                        <el-input v-model="LCPForm.userName"></el-input>
                    </el-form-item>
                    <el-form-item :label="$t('VPN Password')" prop="password">
                        <el-input v-model="LCPForm.password"></el-input>
                    </el-form-item>
                    <el-form-item style="text-align: center">
                        <el-button  @click="removeConfig('LCPForm')">{{$t('VPN Delete')}}</el-button>
                        <el-button type="primary"  @click="sumitConfig('LCPForm')">{{$t('VPN Apply')}}</el-button>
                    </el-form-item>
                </el-form>
            </div>
            <hr>
            <div style="width:50%;margin-left: 100px;margin-top: 15px;text-align: right;">
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

    export default {
        components: {ElButton},
        data() {
            return {
                centerDialogVisible: false,
                activeName : 'status',
                LCPForm: {
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
                    ],
                    addFormName:[
                        {required:true}
                    ],
                    addFormServerIP:[
                        {required:true}
                    ],
                    addFormThreshold:[
                        {required:true}
                    ],
                    addFormInterval:[
                        {required:true}
                    ],
                    addFormUserName:[
                        {required:true}
                    ],
                    addFormPassword:[
                        {required:true}
                    ]
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
                statusLog:' 70% finish module graph FlagDependencyExportsPlugin\n' +
                ' 70% sealing WarnCaseSensitiveModulesPlugin\n' +
                ' 70% chunk graph\n' +
                ' 76% basic chunk optimization EnsureChunkConditionsPlugin\n' +
                ' 76% basic chunk optimization RemoveParentModulesPlugin\n' +
                ' 77% advanced chunk optimization SplitChunksPlugin\n' +
                ' 82% before module ids NamedModulesPlugin\n' +
                ' 84% module id optimization\n' +
                ' 86% after chunk id optimization\n' +
                ' 88% hashing\n' +
                ' 88% content hashing JavascriptModulesPlugin\n' +
                ' 88% content hashing JavascriptModulesPlugin\n' +
                ' 88% content hashing JavascriptModulesPlugin\n' +
                ' 88% content hashing JavascriptModulesPlugin\n' +
                ' 88% content hashing JavascriptModulesPlugin\n' +
                ' 88% content hashing JavascriptModulesPlugin'

            };
        },
        methods: {
            sumitConfig(formName) {
                this.$refs[formName].validate((valid) => {
                    if (valid) {
                        alert('submit!');
                    } else {
                        console.log('error submit!!');
                        return false;
                    }
                });
            },
            removeConfig(formName) {
                this.$refs[formName].resetFields();
            }
        }
    }
</script>


