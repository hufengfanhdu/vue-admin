<template>
    <div id="login">
        <div class="login-wrap">
            <ul class="menu-tab">
                <li :class="{'current': tab.current}"
                    v-for="tab in menuTab"
                    :key="tab.id"
                    @click="handleClick(tab)">{{ tab.txt }}</li>
            </ul>
            <!--表单-->
            <el-form :model="ruleForm" status-icon :rules="rules" ref="ruleForm" class="login-form" size="medium">

                <el-form-item  prop="username">
                    <label>邮箱</label>
                    <el-input type="text" v-model="ruleForm.username" autocomplete="off"></el-input>
                </el-form-item>

                <el-form-item  prop="password">
                    <label>密码</label>
                    <el-input type="password"
                              v-model="ruleForm.password"
                              autocomplete="off"
                              minlength="6"
                              maxlength="20"></el-input>
                </el-form-item>

                <el-form-item  prop="check" v-show="menuTab[1].current">
                    <label>确认密码</label>
                    <el-input type="password"
                              v-model="ruleForm.check"
                              autocomplete="off"
                              minlength="6"
                              maxlength="20"></el-input>
                </el-form-item>

                <el-form-item  prop="code">
                    <label>验证码</label>
                    <el-row :gutter="20">
                        <el-col :span="15">
                            <el-input v-model.number="ruleForm.code"></el-input>
                        </el-col>
                        <el-col :span="9">
                            <el-button type="success" class="block">获取验证码</el-button>
                        </el-col>
                    </el-row>

                </el-form-item>

                <el-form-item>
                    <el-button type="danger" @click="submitForm('ruleForm')" class="block">提交</el-button>
                </el-form-item>
            </el-form>
        </div>
    </div>
</template>

<script>
    import { stripscript, validateEmail, validatePassword, validateCode } from '@/utils/validate';
    export default{
        name: 'login',
        data() {
            // 验证用户名
            var validateUserName = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error('请输入用户名'));
                } else if (!validateEmail(value)) {
                    callback(new Error('用户名格式错误'));
                } else {
                    callback();
                }
            };
            // 验证密码
            var validatePass = (rule, value, callback) => {
                this.ruleForm.password = stripscript(value);
                value = this.ruleForm.password
                if (value === '') {
                    callback(new Error('请输入密码'));
                } else if (!validatePassword(value)) {
                    callback(new Error('请输入6-20位的数字和字母'))
                } else {
                    callback();
                }
            };
            // 验证重复密码
            var validateCheckPass = (rule, value, callback) => {
                if (this.menuTab[0].current === true) callback();
                if (value === '') {
                    callback(new Error('请输入确认密码'));
                } else if (!validatePassword(value)) {
                    callback(new Error('确认密码与密码不相同'));
                } else {
                    callback();
                }
            };
            // 验证验证码
            var checkCode = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error('请输入验证码'));
                } else if (!validateCode(value)) {
                    callback(new Error('验证码格式错误'));
                } else {
                    callback();
                }
            };
            return {
                menuTab: [
                    {txt: '登陆', current: true},
                    {txt: '注册', current: false}
                ],
                ruleForm: {
                    username: '',
                    password: '',
                    check: '',
                    code: ''
                },
                rules: {
                    username: [
                        { validator: validateUserName, trigger: 'blur' }
                    ],
                    password: [
                        { validator: validatePass, trigger: 'blur' }
                    ],
                    check: [
                        { validator: validateCheckPass, trigger: 'blur' }
                    ],
                    code: [
                        { validator: checkCode, trigger: 'blur' }
                    ]
                }
            };
        },
        created() {
        },
        mounted() {
        },
        methods: {
            handleClick: function (tab) {
                this.menuTab.forEach(ele => {
                    ele.current = false;
                })
                tab.current = true;
            },
            submitForm(formName) {
                this.$refs[formName].validate((valid) => {
                    if (valid) {
                        alert('submit!');
                    } else {
                        console.log('error submit!!');
                        return false;
                    }
                });
            }
        },
        props: {

        },
        watch: {

        }
    }
</script>

<style lang="scss" scoped>
    #login {
        height: 100vh;
        background: #3f556b;
    }
    .login-wrap {
        width: 330px;
        margin: auto;
    }
    .menu-tab {
        text-align: center;
        li {
            display: inline-block;
            width: 88px;
            line-height: 36px;
            font-size: 14px;
            color: #fff;
            border-radius: 2px;
            cursor: pointer;
        }
        .current {
            background: rgba(0, 0, 0, .1);
        }
    }
    .login-form {
        margin-top: 29px;
        label {
            display: block;
            font-size: 14px;
            color: white;
            margin-bottom: 3px;
        }
        .item-form {
            margin-bottom: 13px;
        }
        .block {
            display: block;
            width: 100%;
        }
    }
</style>
