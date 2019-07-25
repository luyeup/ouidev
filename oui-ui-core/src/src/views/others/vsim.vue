<template>
    <div>
        <div class="v-align">
            <img src="/icons/point.png" style="height: 15px;"/>
            &nbsp;&nbsp;
            <b>{{$t('VSIM Operator')}}</b>
        </div>
        <div style="width: 90%;height:200px;background-color: white;box-shadow: 0px 3px 0px #bfb2bb;">
            <div style="width: 60%;margin-left: 10px;">
                <br>
                <div class="borderline"></div>
                <div style="margin-left:60px;float: left;height: 50px;line-height: 50px;">
                    {{$t('VSIM Operator')}}
                </div>
                <div style="margin-right:30px;text-align: right;height: 50px;">
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
                <div style="text-align: center">
                    <el-button type="primary"  round>{{$t('VSIM Apply')}}</el-button>
                </div>
            </div>

        </div>
        <br><br>
        <div class="v-align">
            <img src="/icons/point-green.png" style="height: 15px;"/>
            &nbsp;&nbsp;
            <b>{{$t('VSIM AT Command')}}</b>
        </div>
        <div style="width: 90%;padding-bottom:30px;background-color: white;box-shadow: 0px 3px 0px #bfb2bb;">
            <div style="width: 60%;margin-left: 10px;">
                <br>
                <div class="borderline"></div>
                <div style="margin-left:60px;float: left;height: 50px;line-height: 50px;">
                    {{$t('VSIM Shortcut')}}
                </div>
                <div style="margin-right:30px;text-align: right;height: 50px;">
                    <el-select v-model="kvalue" placeholder="select" @change="kChange()">
                        <el-option
                                v-for="item in koptions"
                                :key="item.value"
                                :label="item.label"
                                :value="item.value">
                        </el-option>
                    </el-select>
                </div>
                <div class="borderline"></div>
                <div style="margin-left:60px;float: left;height: 50px;line-height: 50px;">
                    {{$t('VSIM AT Command')}}
                </div>
                <div style="margin-right:30px;text-align: right;height: 50px;">
                    <el-select v-model="atvalue" placeholder="select"  v-if="atIsSelect">
                        <el-option
                                v-for="item in aoptions"
                                :key="item.value"
                                :label="item.label"
                                :value="item.value">
                        </el-option>
                    </el-select>
                    <el-input v-model="atvalue" style="width: 220px;" placeholder="Required" v-if="!atIsSelect"></el-input>
                </div>
                <div class="borderline"></div>
                <div style="margin-left:60px;float: left;height: 50px;line-height: 50px;">
                    {{$t('VSIM Port')}}
                </div>
                <div style="margin-right:30px;text-align: right;height: 50px;">
                    <el-select v-model="pvalue" placeholder="select">
                        <el-option
                                v-for="item in poptions"
                                :key="item.value"
                                :label="item.label"
                                :value="item.value">
                        </el-option>
                    </el-select>
                </div>
                <div class="borderline"></div>
                <div v-if="ATReturnMsg != ''" style="background-color: lavenderblush;">
                    {{ATReturnMsg}}
                </div>
                <br>
                <div style="text-align: center">
                    <el-button type="primary" @click="sendATCommand()" round>{{$t('VSIM Send')}}</el-button>
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
                ATReturnMsg : '',
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
                poptions: [
                    {
                        value: '/dev/ttyUSB0',
                        label: '/dev/ttyUSB0'
                    },{
                        value: '/dev/ttyUSB1',
                        label: '/dev/ttyUSB1'
                     }, {
                    value: '/dev/ttyUSB2',
                    label: '/dev/ttyUSB2'
                },
                    {
                        value: '/dev/ttyUSB3',
                        label: '/dev/ttyUSB3'
                    }
                ],
                kvalue:'',
                atvalue:'',
                pvalue:'',
                value:''

            }
        },
        created(){ //页面加载时执行

        },
        mounted(){  //页面加载完成后执行

        },
        methods:{
            kChange(){
                console.log(this.kvalue);
                if (this.kvalue==1){
                    this.atIsSelect=false;
                    this.atvalue = '';
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
            },
            sendATCommand(){
//                this.atvalue  //指令
//                this.pvalue  //端口
                this.ATReturnMsg = "success"; //指令执行的返回结果
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
    .borderline{
        border-top: 0.5px solid #e5e5e5;
        padding-top: 10px;
        margin-top: 10px;
    }
</style>