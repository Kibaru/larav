<template>
    <div class="login row justify-content-center">
        <div class="col-md-6">
            <div v-if="registeredUser" class="text-success">Thank you {{registeredUser.name}}.You can now login</div>
            <div class="card">
                <div class="card-header">
                   <h3>Login</h3> 
                </div>
                <div class="card-body">
                    <form @submit.prevent="authenticate">
                        <div class="form-group row" v-if="authError">
                            <p class="error">
                                {{authError}}
                            </p>
                        </div>
                        <div class="form-group row">
                            <label for="email">Email</label>
                            <input type="email" class="form-control" v-model="formLogin.email" placeholder="Email address">
                        </div>
                        <div class="form-group row">
                            <label for="password">Password</label>
                            <input type="password" class="form-control" v-model="formLogin.password" placeholder="password">
                        </div>
                        <div class="form-group row">
                            <input type="submit" value="Login" class="btn btn-outline-primary ml-auto">
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import {login} from '../partials/auth';
export default {
    data(){
        return {
            formLogin: {
            email: '',
            password: ''
        },
        error: null
        }
    },
    methods:{
        authenticate(){
            this.$store.dispatch('login');

        login(this.$data.formLogin)
            .then(res => {
                this.$store.commit("loginSuccess", res);
                this.$router.push({path: '/dashboard'});
            })
            .catch(error => {
                this.$store.commit("loginFailed", {error});
            })
        }
    },
    computed:{
        authError(){
            return this.$store.getters.authError
        },
        registeredUser(){
            return this.$store.getters.registeredUser
        }
    }
}
</script>

<style scoped>
.error{
    text-align: center;
    color: red;
}
</style>