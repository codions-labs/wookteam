<template>
    <div class="w-box index">

        <v-title>{{$L('轻量级的团队在线协作')}}</v-title>

        <div class="header">
            <div class="z-row">
                <div class="header-col-sub">
                    <h2>
                        <img v-if="systemConfig.logo" :src="systemConfig.logo">
                        <img v-else src="../../../statics/images/logo-white.png">
                        <span>{{$L('轻量级的团队在线协作')}}</span>
                    </h2>
                </div>
                <div class="z-1">
                    <dl>
                        <dd>
                            <a v-if="systemConfig.github=='show'" class="right-info" target="_blank" href="https://github.com/kuaifan/wookteam">
                                <Icon class="right-icon" type="logo-github"/>
                            </a>
                            <Dropdown class="right-info" trigger="hover" @on-click="setLanguage" transfer>
                                <div>
                                    <Icon class="right-icon" type="md-globe"/>
                                    <Icon type="md-arrow-dropdown"/>
                                </div>
                                <Dropdown-menu slot="list">
                                    <Dropdown-item name="zh" :selected="getLanguage() === 'zh'">中文</Dropdown-item>
                                    <Dropdown-item name="en" :selected="getLanguage() === 'en'">English</Dropdown-item>
                                </Dropdown-menu>
                            </Dropdown>
                        </dd>
                    </dl>
                </div>
            </div>
        </div>

        <div class="welcome">
            <div class="banner">
                <div class="z-row">
                    <div class="z-16">
                        <Carousel class="banner-carousel" autoplay loop :autoplay-speed="5000">
                            <CarouselItem>
                                <img src="../../../statics/images/index/banner/1.jpg">
                            </CarouselItem>
                            <CarouselItem>
                                <img src="../../../statics/images/index/banner/2.jpg">
                            </CarouselItem>
                        </Carousel>
                    </div>
                    <div class="z-8"><h3>{{$L('酷团队协作工具就从这里开始')}}</h3>
                        <div class="bl inline-block">
                            <span class="start" @click="loginChack">{{$L('立即登陆')}}</span>
                        </div>
                    </div>
                </div>
            </div>
            <div class="second">
                <div class="bg"></div>
                <div class="z-row">
                    <div class="z-6"><a href="#W_link1"><i class="ft icon">&#xe753;</i>{{$L('待办四象限')}}</a></div>
                    <div class="z-6"><a href="#W_link2"><i class="ft icon">&#xe6b8;</i>{{$L('项目管理')}}</a></div>
                    <div class="z-6"><a href="#W_link3"><i class="ft icon">&#xe915;</i>{{$L('在线知识库')}}</a></div>
                    <div class="z-6"><a href="#W_link4"><i class="ft icon">&#xe706;</i>{{$L('日程管理')}}</a></div>
                </div>
            </div>
        </div>

        <div class="z-row block">
            <div class="z-6">
                <div class="wrap-left" id="W_link1"><i class="ft icon">&#xe753;</i>{{$L('待办四象限：突出事情优先级，帮助员工合理安排时间，提高工作效率。')}}</div>
            </div>
            <div class="z-18"><img src="../../../statics/images/index/todo.jpg"/></div>
        </div>
        <div class="z-row block">
            <div class="z-18"><img src="../../../statics/images/index/project.jpg"/></div>
            <div class="z-6">
                <div class="wrap-right" id="W_link2"><i class="ft icon">&#xe6b8;</i>{{$L('项目管理：自定义项目看板，可视化任务安排。')}}</div>
            </div>
        </div>
        <div class="z-row block">
            <div class="z-6">
                <div class="wrap-left" id="W_link3"><i class="ft icon">&#xe915;</i>{{$L('在线知识库：在线流程图，在线文档，以及可视化的目录编排，文档管理无忧。')}}</div>
            </div>
            <div class="z-18"><img src="../../../statics/images/index/wiki.jpg"/></div>
        </div>
        <div class="z-row block">
            <div class="z-18"><img src="../../../statics/images/index/week.jpg"/></div>
            <div class="z-6">
                <div class="wrap-right" id="W_link4"><i class="ft icon">&#xe706;</i>{{$L('日程管理：可视化日程管理，快速搞定工作计划，了解工作宏观安排。')}}</div>
            </div>
        </div>

        <div class="p-footer"><span>WookTeam &copy; 2018-2020</span></div>

        <Modal
            v-model="loginShow"
            :mask-closable="false"
            class-name="simple-modal">
            <Form ref="login" :model="formLogin" :rules="ruleLogin">
                <FormItem prop="username">
                    <Input type="text" v-model="formLogin.username" :placeholder="$L('用户名')" @on-enter="onLogin">
                        <Icon type="ios-person-outline" slot="prepend"></Icon>
                    </Input>
                </FormItem>
                <FormItem prop="userpass">
                    <Input type="password" v-model="formLogin.userpass" :placeholder="$L('密码')" @on-enter="onLogin">
                        <Icon type="ios-lock-outline" slot="prepend"></Icon>
                    </Input>
                </FormItem>
                <FormItem v-if="loginType=='reg'" prop="userpass2">
                    <Input type="password" v-model="formLogin.userpass2" :placeholder="$L('确认密码')" @on-enter="onLogin">
                        <Icon type="ios-lock-outline" slot="prepend"></Icon>
                    </Input>
                </FormItem>
            </Form>
            <div slot="header" class="login-header">
                <div @click="loginType='login'" class="login-header-item" :class="{active:loginType=='login'}">{{$L('用户登录')}}</div>
                <div v-if="systemConfig.reg=='open'" @click="loginType='reg'" class="login-header-item" :class="{active:loginType=='reg'}">{{$L('注册账号')}}</div>
            </div>
            <div slot="footer">
                <Button type="default" @click="loginShow=false">{{$L('取消')}}</Button>
                <Button type="primary" :loading="loadIng > 0" @click="onLogin">{{$L(loginType=='reg'?'注册':'登录')}}</Button>
            </div>
        </Modal>
    </div>
</template>

<style lang="scss">
    .login-header {
        display: flex;
        align-items: center;
        .login-header-item {
            height: 20px;
            line-height: 20px;
            font-size: 14px;
            color: #444444;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
            padding-right: 12px;
            cursor: pointer;
            &.active {
                font-size: 16px;
                color: #17233d;
                font-weight: 500;
            }
        }
    }
</style>
<style lang="scss" scoped>
    .index {
        position: absolute;
        color: #000000;
        top: 0;
        left: 0;
        min-width: 100%;
        min-height: 100%;
        padding: 0;
        margin: 0;
        .header {
            height: 50px;
            background: #0396f2;
            padding-top: 12px;
            max-width: 1280px;
            margin: 0 auto;
            .z-row {
                color: #fff;
                height: 50px;
                position: relative;
                z-index: 2;
                max-width: 1680px;
                margin: 0 auto;
                .header-col-sub {
                    width: 500px;
                    h2 {
                        position: relative;
                        padding: 1rem 0 0 1rem;
                        display: flex;
                        align-items: flex-end;
                        img {
                            width: 150px;
                            margin-right: 6px;
                        }
                        span {
                            font-size: 12px;
                            font-weight: normal;
                            color: rgba(255, 255, 255, 0.85);
                            line-height: 14px;
                        }
                    }
                }
                .z-1 {
                    dl {
                        position: absolute;
                        right: 20px;
                        top: 0;
                        font-size: 14px;
                        dd {
                            line-height: 50px;
                            color: #fff;
                            cursor: pointer;
                            margin-right: 1px;
                            .right-info {
                                display: inline-block;
                                cursor: pointer;
                                margin-left: 12px;
                                color: #ffffff;
                                .right-icon {
                                    font-size: 26px;
                                    vertical-align: middle;
                                }
                            }
                        }
                    }
                }
            }
        }
        .welcome {
            height: 700px;
            display: block;
            background: #2d8cf0;
            overflow: hidden;
            color: #FFFFFF;
            padding-top: 480px;
            margin-top: -480px;
            transform: skewY(-2deg);
            box-shadow: 0 2px 244px 0 rgba(56, 132, 255, 0.4);
            .unslider-arrow {
                display: none;
            }
            .banner {
                padding-top: 60px;
                height: 460px;
                max-width: 1200px;
                margin: 0 auto;
                transform: skewY(2deg);
                .banner-carousel {
                    max-width: 685px;
                }
                img {
                    height: 400px;
                    border: 5px solid #fff;
                    border-radius: 5px;
                }
            }
            .z-8 {
                text-align: center;
            }
            h3 {
                color: rgba(255, 255, 255, 0.8);
                font-size: 40px;
                font-weight: normal;
                text-align: center;
                margin: 30px auto;
                width: 380px;
            }
            .start {
                display: inline-block;
                width: 160px;
                height: 50px;
                line-height: 50px;
                text-align: center;
                font-weight: normal;
                cursor: pointer;
                font-size: 20px;
                background: #fff;
                color: #0396f2;
                border-radius: 4px;
                border: 0;

                &:hover, &:focus {
                    background: #f6f6f6;
                    color: #0396f2;
                }
            }
            .second {
                position: relative;
                height: 220px;
                text-align: center;
                transform: skewY(2deg);

                .bg {
                    transform: skewY(-4.5deg);
                    display: block;
                    position: absolute;
                    top: 0;
                    left: 0;
                    right: 0;
                    bottom: 0;
                    background: #1F65D6;
                }
                .z-row {
                    z-index: 2;
                    position: relative;
                    font-size: 22px;
                    max-width: 1400px;
                    margin: 0 auto;
                    line-height: 220px;
                }
                i {
                    color: rgba(255, 255, 255, 0.85);
                    margin-right: 6px;
                }
                a {
                    color: #fff;
                    &:hover, &:visited {
                        color: #fff;
                    }
                }

            }
        }

        .block {
            max-width: 1200px;
            margin: 30px auto;
            padding-top: 50px;
            border: 1px solid #F4F7F9;

            .wrap-left, .wrap-right {
                line-height: 36px;
                color: #666;
                font-size: 16px;
            }
            .wrap-left {
                margin: 20px 30px 0 0;
            }
            .wrap-right {
                margin: 20px 0 0 30px;
            }
            i {
                color: rgba(248, 14, 21, 0.7);
                margin-right: 6px;
            }
            img {
                border: 5px solid #fff;
                border-radius: 10px;
                width: 100%;
            }
        }

        .p-footer {
            margin: 20px 0;
            text-align: center;
            color: #333;

            a, span {
                color: #333;
                margin-left: 10px;
            }
        }
    }
</style>
<script>
    export default {
        data () {
            return {
                loadIng: 0,
                loginShow: false,
                loginType: 'login',

                formLogin: {
                    username: '',
                    userpass: '',
                    userpass2: ''
                },
                ruleLogin: {},

                systemConfig: $A.jsonParse($A.storage("systemSetting"), {
                    logo: '',
                    github: '',
                    reg: '',
                }),

                fromUrl: '',
            }
        },
        created() {
            this.ruleLogin = {
                username: [
                    { required: true, message: this.$L('请填写用户名！'), trigger: 'change' },
                    { type: 'string', min: 2, message: this.$L('用户名长度至少2位！'), trigger: 'change' }
                ],
                userpass: [
                    { required: true, message: this.$L('请填写登录密码！'), trigger: 'change' },
                    { type: 'string', min: 6, message: this.$L('密码错长度至少6位！'), trigger: 'change' }
                ],
                userpass2: [
                    { required: true, message: this.$L('请填写确认密码！'), trigger: 'change' },
                    { type: 'string', min: 6, message: this.$L('确认密码错长度至少6位！'), trigger: 'change' },
                    {
                        validator: (rule, value, callback) => {
                            if (value !== this.formLogin.userpass) {
                                callback(new Error(this.$L('两次密码输入不一致！')));
                            } else {
                                callback();
                            }
                        },
                        required: true,
                        trigger: 'change'
                    },
                ]
            };
        },
        mounted() {
            this.getSetting();
            this.fromUrl = decodeURIComponent($A.getObject(this.$route.query, 'from'));
            if (this.fromUrl) {
                this.loginChack();
            }
        },
        deactivated() {
            this.loginShow = false;
        },
        watch: {
            loginShow(val) {
                if (val) {
                    this.getSetting();
                } else {
                    this.loginType = 'login';
                }
            }
        },
        methods: {
            getSetting() {
                $A.apiAjax({
                    url: 'system/setting',
                    error: () => {
                        $A.storage("systemSetting", {});
                    },
                    success: (res) => {
                        if (res.ret === 1) {
                            this.systemConfig = res.data;
                            this.systemConfig.github = this.systemConfig.github || 'show';
                            this.systemConfig.reg = this.systemConfig.reg || 'open';
                            $A.storage("systemSetting", this.systemConfig);
                        } else {
                            $A.storage("systemSetting", {});
                        }
                    }
                });
            },

            loginChack() {
                if ($A.getToken() !== false) {
                    this.goForward({path: '/todo'}, true);
                } else {
                    this.loginShow = true;
                }
            },
            onLogin() {
                this.$refs.login.validate((valid) => {
                    if (valid) {
                        this.loadIng++;
                        $A.ajax({
                            url: $A.apiUrl('users/login?type=' + this.loginType),
                            data: this.formLogin,
                            complete: () => {
                                this.loadIng--;
                            },
                            success: (res) => {
                                if (res.ret === 1) {
                                    $A.storage("userInfo", res.data);
                                    $A.setToken(res.data.token);
                                    $A.triggerUserInfoListener(res.data);
                                    //
                                    this.loadIng--;
                                    this.loginShow = false;
                                    this.$refs.login.resetFields();
                                    this.$Message.success(this.$L('登录成功'));
                                    if (this.fromUrl) {
                                        window.location.replace(this.fromUrl);
                                    } else {
                                        this.goForward({path: '/todo'}, true);
                                    }
                                } else {
                                    this.$Modal.error({
                                        title: this.$L("温馨提示"),
                                        content: res.msg
                                    });
                                }
                            }
                        })
                    }
                })
            }
        },
    }
</script>
