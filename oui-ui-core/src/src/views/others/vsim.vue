<template>
    <div>
        <div class="v-align">
            <img src="/icons/point.png" style="height: 15px;"/>
            &nbsp;&nbsp;
            <b>运营商</b>
        </div>
        <div style="width: 90%;height:200px;background-color: white;box-shadow: 0px 3px 0px #bfb2bb;">
            <div style="width: 60%;margin-left: 10px;">
                <br>
                <hr>
                <div style="margin-left:60px;float: left;height: 50px;line-height: 50px;">
                    运营商
                </div>
                <div style="margin-right:30px;text-align: right;height: 50px;">
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
                <div style="text-align: center">
                    <el-button type="primary"  round>应用</el-button>
                </div>
            </div>

        </div>
        <br><br>
        <div class="v-align">
            <img src="/icons/point-green.png" style="height: 15px;"/>
            &nbsp;&nbsp;
            <b>AT指令</b>
        </div>
        <div style="width: 90%;padding-bottom:30px;background-color: white;box-shadow: 0px 3px 0px #bfb2bb;">
            <div style="width: 60%;margin-left: 10px;">
                <br>
                <hr>
                <div style="margin-left:60px;float: left;height: 50px;line-height: 50px;">
                    快捷方式
                </div>
                <div style="margin-right:30px;text-align: right;height: 50px;">
                    <el-select v-model="kvalue" placeholder="请选择" @change="kChange()">
                        <el-option
                                v-for="item in koptions"
                                :key="item.value"
                                :label="item.label"
                                :value="item.value">
                        </el-option>
                    </el-select>
                </div>
                <hr>
                <div style="margin-left:60px;float: left;height: 50px;line-height: 50px;">
                    AT指令
                </div>
                <div style="margin-right:30px;text-align: right;height: 50px;">
                    <el-select v-model="atvalue" placeholder="请选择"  v-if="atIsSelect">
                        <el-option
                                v-for="item in aoptions"
                                :key="item.value"
                                :label="item.label"
                                :value="item.value">
                        </el-option>
                    </el-select>
                    <el-input v-model="atvalue" style="width: 220px;" v-if="!atIsSelect"></el-input>
                </div>
                <hr>
                <div style="margin-left:60px;float: left;height: 50px;line-height: 50px;">
                    端口
                </div>
                <div style="margin-right:30px;text-align: right;height: 50px;">
                    <el-select v-model="pvalue" placeholder="请选择">
                        <el-option
                                v-for="item in poptions"
                                :key="item.value"
                                :label="item.label"
                                :value="item.value">
                        </el-option>
                    </el-select>
                </div>
                <hr>
                <br>
                <div style="text-align: center">
                    <el-button type="primary"  round>发送</el-button>
                </div>
            </div>
        </div>
    </div>


</template>
<script>
    export default {
        data() {
            return {
                atIsSelect:true,
                options: [{
                        value: '1',
                        label: '中国移动'
                     }, {
                        value: '2',
                        label: '中国联通'
                    },
                    {
                        value: '3',
                        label: '中国电信'
                    }
                ],
                koptions: [{
                    value: '1',
                    label: 'Manual command'
                }, {
                    value: '2',
                    label: 'Request IMEI'
                },
                    {
                        value: '3',
                        label: 'Request QCCID'
                    },
                    {
                        value: '4',
                        label: 'Request IMSI'
                    },
                    {
                        value: '5',
                        label: 'Check Signal Quality'
                    },
                    {
                        value: '6',
                        label: 'Reset modem'
                    },
                    {
                        value: '7',
                        label: 'Operator Names'
                    }
                ],
                aoptions: [{
                    value: 'AT+GSN',
                    label: 'AT+GSN'
                }, {
                    value: 'AT+QCCID',
                    label: 'AT+QCCID'
                },
                    {
                        value: 'AT+CIMI',
                        label: 'AT+CIMI'
                    },
                    {
                        value: 'AT+CSQ',
                        label: 'AT+CSQ'
                    },
                    {
                        value: 'AT&F0',
                        label: 'AT&F0'
                    },
                    {
                        value: 'AT+COPS',
                        label: 'AT+COPS'
                    }
                ],
                poptions: [{
                    value: '/dev/ttyUSB1',
                    label: '/dev/ttyUSB1'
                }, {
                    value: '/dev/ttyUSB2',
                    label: '/dev/ttyUSB2'
                }
                ],
                kvalue:'',
                atvalue:'',
                pvalue:'',
                value:''

            }
        },
        methods:{
            kChange(){
                console.log(this.kvalue);
                if (this.kvalue==1){
                    this.atIsSelect=false;
                } else if(this.kvalue==2) {
                    this.atIsSelect=true;
                    this.atvalue='AT+GSN';
                }
                else if(this.kvalue==3) {
                    this.atIsSelect=true;
                    this.atvalue='AT+QCCID';
                }
                else if(this.kvalue==4) {
                    this.atIsSelect=true;
                    this.atvalue='AT+CIMI';
                }
                else if(this.kvalue==5) {
                    this.atIsSelect=true;
                    this.atvalue='AT+CSQ';
                }
                else if(this.kvalue==6) {
                    this.atIsSelect=true;
                    this.atvalue='AT&F0';
                }
                else if(this.kvalue==7) {
                    this.atIsSelect=true;
                    this.atvalue='AT+COPS';
                }
            }
        }
    }
</script>
<style>
    .v-align {
        margin: 0 auto;
        height: 40px;
        line-height: 40px;
        padding-left: 10px;
    }
</style>