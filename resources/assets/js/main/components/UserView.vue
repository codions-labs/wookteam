<template>
    <div class="user-view-inline">
        <div class="user-view-info">
            <UserImg v-if="showimg" class="user-view-img" :info="userInfo"/>
            <Tooltip :disabled="loadIng" :delay="delay" :transfer="transfer" :placement="placement" maxWidth="auto" @on-popper-show="popperShow">
                {{nickname || username}}
                <div slot="content" style="white-space:normal">
                    <div>{{$L('用户名')}}: {{username}}</div>
                    <div>{{$L('职位/职称')}}: {{profession || '-'}}</div>
                </div>
            </Tooltip>
        </div>
    </div>
</template>

<style lang="scss">
    .user-view-inline {
        .user-view-info {
            .ivu-tooltip {
                max-width: 100%;
                display: flex;
                flex-direction: column;
                justify-content: center;
                .ivu-tooltip-rel {
                    max-width: 100%;
                    white-space: nowrap;
                    overflow: hidden;
                    text-overflow: ellipsis;
                }
            }
            .user-view-img {
                .usertext-container-text {
                    transform: scale(0.86);
                }
            }
        }
    }
</style>
<style lang="scss" scoped>
    .user-view-inline {
        display: inline-block;
        max-width: 100%;
        .user-view-info {
            display: flex;
            align-items: center;
            .user-view-img {
                width: 16px;
                height: 16px;
                font-size: 12px;
                line-height: 16px;
                border-radius: 50%;
                margin-right: 3px;
            }
            .user-view-title {
                flex: 1;
                line-height: 1.2;
            }
        }
    }
</style>

<script>
    export default {
        name: 'UserView',
        props: {
            username: {
                default: ''
            },
            delay: {
                type: Number,
                default: 600
            },
            transfer: {
                type: Boolean,
                default: true
            },
            placement: {
                default: 'bottom'
            },
            showimg: {
                type: Boolean,
                default: false
            },
            info: {
                default: null
            },
        },
        data() {
            return {
                loadIng: true,

                nickname: null,
                userimg: '',
                profession: ''
            }
        },
        mounted() {
            this.getUserData(300);
        },
        watch: {
            username() {
                this.getUserData(300);
            },
            info: {
                handler() {
                    this.upInfo()
                },
                deep: true
            }
        },
        computed: {
            userInfo() {
                const {username, nickname, userimg} = this;
                return {username, nickname, userimg}
            }
        },
        methods: {
            isJson(obj) {
                return typeof (obj) == "object" && Object.prototype.toString.call(obj).toLowerCase() == "[object object]" && typeof obj.length == "undefined";
            },

            upInfo() {
                if (this.isJson(this.info)) {
                    this.$set(this.info, 'nickname', this.nickname);
                    this.$set(this.info, 'userimg', this.userimg);
                }
            },

            popperShow() {
                this.getUserData(30)
            },

            getUserData(cacheTime) {
                $A.getUserBasic(this.username, (data, success) => {
                    if (success) {
                        this.nickname = data.nickname;
                        this.userimg = data.userimg;
                        this.profession = data.profession;
                    } else {
                        this.nickname = '';
                        this.userimg = '';
                        this.profession = '';
                    }
                    this.loadIng = false;
                    this.$emit("on-result", data);
                    this.upInfo();
                }, cacheTime);
            }
        }
    }
</script>
