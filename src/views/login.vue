<style lang="less">
    @import './login.less';
</style>

<template>
    <div class="login" @keydown.enter="handleSubmit">
        <div class="login-con">
            <Card :bordered="false">
                <p slot="title">
                    <Icon type="log-in"></Icon>
                    欢迎登录
                </p>
                <div class="form-con">
                    <Form ref="loginForm" :model="form" :rules="rules">
                        <FormItem prop="name">
                            <Input v-model="form.name" placeholder="请输入用户名">
                                <span slot="prepend">
                                    <Icon :size="16" type="person"></Icon>
                                </span>
                            </Input>
                        </FormItem>
                        <FormItem prop="pwd">
                            <Input type="password" v-model="form.pwd" placeholder="请输入密码">
                                <span slot="prepend">
                                    <Icon :size="14" type="locked"></Icon>
                                </span>
                            </Input>
                        </FormItem>
                        <FormItem>
                            <Button @click="handleSubmit" type="primary" long>登录</Button>
                        </FormItem>
                    </Form>
                    <p class="login-tip" v-if="status == 3002" style="color:red">
                        {{ msg }}
                    </p>
                    <p class="login-tip" v-if="status != 3002">
                        {{ msg }}
                    </p>
                </div>
            </Card>
        </div>
    </div>
</template>

<script>
import Cookies from 'js-cookie';
import axios from 'axios';
export default {
    data () {
        return {
            status: 0,
            msg: '输入用户名和密码登录',
            form: {
                name: 'iview_admin',
                pwd: ''
            },
            rules: {
                name: [
                    { required: true, message: '账号不能为空', trigger: 'blur' }
                ],
                pwd: [
                    { required: true, message: '密码不能为空', trigger: 'blur' }
                ]
            }
        };
    },
    methods: {
        handleSubmit () {
            // this.$refs.loginForm.validate((valid) => {
            //     if (valid) {
            //         Cookies.set('user', this.form.name);
            //         Cookies.set('password', this.form.pwd);
            //         this.$store.commit('setAvator', 'https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=3448484253,3685836170&fm=27&gp=0.jpg');
            //         if (this.form.name === 'iview_admin') {
            //             Cookies.set('access', 0);
            //         } else {
            //             Cookies.set('access', 1);
            //         }
            //         // this.$router.push({
            //         //     name: 'home_index'
            //         // });
            //     }
            // });
            this.msg = ''
            axios.post('/api/user/login', this.form,{
                    headers: {
                            'Content-Type': 'application/json'
                      }
                  }).then((response)=> {

                    let code = response.data.code
                    this.status = code
                    if (code == 2000) {
                        console.log("success")
                        let name = response.data.data.name
                        let pwd = response.data.data.pwd
                        Cookies.set('user', name);
                        Cookies.set('password', pwd);
                        this.$store.commit('setAvator', 'https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=3448484253,3685836170&fm=27&gp=0.jpg');
                        if (name === 'admin') {
                            Cookies.set('access', 0);
                        } else {
                            Cookies.set('access', 1);
                        }
                        this.$router.push({
                            name: 'home_index'
                        });
                    } else if (code == 3002){
                        this.msg = "用户不存在!"
                        console.log("用户不存在！")
                    } else {
                        console.log("error")
                    }
                  }).catch((error) => {
                    console.log(error);
                  });
        }
    }
};
</script>

<style>

</style>
