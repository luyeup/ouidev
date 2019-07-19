<template>
<div>
    <br>
    <el-tabs v-model="activeName" type="card">
        <el-tab-pane label="状态" name="status">
            <div style="width: 90%;">
                <hr>
                <div style="float: left;padding-top: 6px;">
                    <span>名称</span>
                </div>
                <div style="text-align: right;">
                    <el-select v-model="value" placeholder="请选择">
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
            </div>
        </el-tab-pane>
        <el-tab-pane label="管理" name="manager">
            <div style="width:50%;margin-left: 100px;margin-top: 30px;">
                <el-form :model="LCPForm" :rules="rules" ref="ruleForm" label-width="140px" class="demo-ruleForm">
                    <el-form-item label="服务器" prop="serverIP">
                        <el-input v-model="LCPForm.serverIP"></el-input>
                    </el-form-item>
                    <el-form-item label="LCP响应故障阈值" prop="threshold">
                        <el-input v-model="LCPForm.threshold"></el-input>
                    </el-form-item>
                    <el-form-item label="LCP响间隔" prop="interval">
                        <el-input v-model="LCPForm.interval"></el-input>
                    </el-form-item>
                    <el-form-item label="用户名" prop="userName">
                        <el-input v-model="LCPForm.userName"></el-input>
                    </el-form-item>
                    <el-form-item label="密码" prop="password">
                        <el-input v-model="LCPForm.password"></el-input>
                    </el-form-item>
                    <!--<el-form-item label="活动区域" prop="region">-->
                    <!--<el-select v-model="ruleForm.region" placeholder="请选择活动区域">-->
                    <!--<el-option label="区域一" value="shanghai"></el-option>-->
                    <!--<el-option label="区域二" value="beijing"></el-option>-->
                    <!--</el-select>-->
                    <!--</el-form-item>-->
                    <!--<el-form-item label="活动时间" required>-->
                    <!--<el-col :span="11">-->
                    <!--<el-form-item prop="date1">-->
                    <!--<el-date-picker type="date" placeholder="选择日期" v-model="ruleForm.date1" style="width: 100%;"></el-date-picker>-->
                    <!--</el-form-item>-->
                    <!--</el-col>-->
                    <!--<el-col class="line" :span="2">-</el-col>-->
                    <!--<el-col :span="11">-->
                    <!--<el-form-item prop="date2">-->
                    <!--<el-time-picker type="fixed-time" placeholder="选择时间" v-model="ruleForm.date2" style="width: 100%;"></el-time-picker>-->
                    <!--</el-form-item>-->
                    <!--</el-col>-->
                    <!--</el-form-item>-->
                    <!--<el-form-item label="即时配送" prop="delivery">-->
                    <!--<el-switch v-model="ruleForm.delivery"></el-switch>-->
                    <!--</el-form-item>-->
                    <!--<el-form-item label="活动性质" prop="type">-->
                    <!--<el-checkbox-group v-model="ruleForm.type">-->
                    <!--<el-checkbox label="美食/餐厅线上活动" name="type"></el-checkbox>-->
                    <!--<el-checkbox label="地推活动" name="type"></el-checkbox>-->
                    <!--<el-checkbox label="线下主题活动" name="type"></el-checkbox>-->
                    <!--<el-checkbox label="单纯品牌曝光" name="type"></el-checkbox>-->
                    <!--</el-checkbox-group>-->
                    <!--</el-form-item>-->
                    <!--<el-form-item label="特殊资源" prop="resource">-->
                    <!--<el-radio-group v-model="ruleForm.resource">-->
                    <!--<el-radio label="线上品牌商赞助"></el-radio>-->
                    <!--<el-radio label="线下场地免费"></el-radio>-->
                    <!--</el-radio-group>-->
                    <!--</el-form-item>-->
                    <!--<el-form-item label="活动形式" prop="desc">-->
                    <!--<el-input type="textarea" v-model="ruleForm.desc"></el-input>-->
                    <!--</el-form-item>-->
                    <el-form-item style="text-align: center">
                        <el-button  @click="removeConfig('LCPForm')">删除</el-button>
                        <el-button type="primary"  @click="sumitConfig('LCPForm')">应用</el-button>
                    </el-form-item>
                </el-form>
            </div>
            <hr>
            <div style="width:50%;margin-left: 100px;margin-top: 15px;text-align: right;">
                <el-button type="primary" style="width: 78%;" round @click="centerDialogVisible = true">+增加一个服务器配置</el-button>
            </div>
            <el-dialog
                    style=""
                    title="添加服务器配置"
                    :visible.sync="centerDialogVisible"
                    width="50%"
                    center
                    >
                <div  style="text-align: center">
                    <el-form :model="addForm" :rules="rules" ref="ruleForm" label-width="140px" class="demo-ruleForm">
                        <el-form-item label="名称" prop="addFormName">
                            <el-input v-model="addForm.name" ></el-input>
                        </el-form-item>
                        <el-form-item label="服务器" prop="addFormServerIP">
                            <el-input v-model="addForm.serverIP"></el-input>
                        </el-form-item>
                        <el-form-item label="LCP响应故障阈值" prop="addFormThreshold">
                            <el-input v-model="addForm.threshold"></el-input>
                        </el-form-item>
                        <el-form-item label="LCP响间隔" prop="addFormInterval">
                            <el-input v-model="addForm.interval"></el-input>
                        </el-form-item>
                        <el-form-item label="用户名" prop="addFormUserName">
                            <el-input v-model="addForm.userName"></el-input>
                        </el-form-item>
                        <el-form-item label="密码" prop="addFormPassword">
                            <el-input v-model="addForm.password"></el-input>
                        </el-form-item>
                    </el-form>
                    <span slot="footer" class="dialog-footer">
                        <el-button @click="centerDialogVisible = false">取 消</el-button>
                        <el-button type="primary"  @click="sumitConfig('addForm')" >添加</el-button>
                    </span>
                </div>
            </el-dialog>



        </el-tab-pane>
    </el-tabs>

</div>

</template>
<script>
    export default {
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


