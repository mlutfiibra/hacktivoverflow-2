<template>
    <div class="card login-form">
        <h3 class="mb-3 text-center">Login</h3>
        <form @submit.prevent="login">
            <div class="form-group">
                <label for="exampleInputEmail1">Email address</label>
                <input 
                    type="email" 
                    class="form-control" 
                    id="exampleInputEmail1"
                    v-model="form.email"
                    aria-describedby="emailHelp"
                    placeholder="Enter email"
                >
                <small id="emailHelp" class="form-text text-muted">We'll never share your email with anyone else.</small>
            </div>
            <div class="form-group">
                <label for="exampleInputPassword1">Password</label>
                <input 
                    type="password" 
                    class="form-control" 
                    id="exampleInputPassword1" 
                    v-model="form.password"                    
                    placeholder="Password"
                >
            </div>
            <div class="form-group form-check">
                <input type="checkbox" class="form-check-input" id="exampleCheck1">
                <label class="form-check-label" for="exampleCheck1">Remember me</label>
            </div>
            <button type="submit" class="btn btn-primary">Login</button>
        </form>
    </div>
</template>

<script>
import myaxios from '@/api/axios'

export default {
    data() {
        return {
            form: {
                email: '',
                password: ''
            }
        }
    },
    methods: {
        login() {
            myaxios
            .post('/login', this.form)
            .then(({ data }) => {
                localStorage.token = data.token

                this.$store.dispatch('setLoggedInUser')
                .then(() => {
                    if(!this.$store.getters.error){
                        this.$store.dispatch('fetchMyQuestions')
                        .then(() => {
                            if(!this.$store.getters.error) {
                                this.$store.dispatch('getWatchedTagsQuestions', this.watchedTags)
                                .then(()=> {
                                    if(!this.$store.getters.error) {
                                        setTimeout(() => {
                                            this.$store.state.isLogin = true
                                            this.$alertify.success(`Welcome ${data.username}`);
                                            this.$router.push('/')
                                        }, 1000);
                                    }
                                })
                            }
                        })
                    }
                })

            })
            .catch(error => {
                this.$alertify.error(error.response.data.msg);
            })
        }
    },
    computed: {
        isLogin() {
            return this.$store.state.isLogin
        }
    }
}
</script>

<style>
.login-form {
    padding: 30px;
    margin: 0px 170px;
}
</style>
