<template>
    <div>
        <nav class="navbar fixed-top navbar-light bg-white" style="padding: .5rem 5.6em; border-bottom: solid 1px #eee;">
            <router-link to="/" class="nav-container"> 
                <img 
                    src="../assets/icon/iron-man-icon_b&w.png" 
                    alt="icon" 
                    style="width: 3em;
                        margin-right: .5em;"
                ><span style="color: #616161;"> <span style="font-size: 18px; font-weight: 600;">hiJarvis</span><span style="display:block; margin-top:-7px;"><small>ask me anything</small></span></span>
            </router-link>
            <div v-if="!isLogin">
                <router-link to="/register">
                    <button type="button" class="btn btn-outline-secondary mr-3">Register</button>
                </router-link>
                <router-link to="/login">
                    <button type="button" class="btn btn-link" style="color: #616161;">Login</button>
                </router-link>
            </div>
            <div v-else>
                <div class="nav-item dropdown">
                    <span 
                        id="navbarDropdown" 
                        class="nav-link dropdown-toggle" 
                        style="color: #616161; cursor:pointer;"
                        role="button" 
                        data-toggle="dropdown" 
                        aria-haspopup="true" 
                        aria-expanded="false"
                    >
                    {{loginUser.username}}
                    </span>
                    <div class="dropdown-menu" aria-labelledby="navbarDropdown">
                        <span @click="logout">Logout</span>
                    </div>
                </div>
            </div>
        </nav>
        <div style="margin-bottom: 4.5em;">&nbsp;</div>
    </div>
</template>

<script>
import { mapState } from 'vuex';
export default {
    computed: {
        ...mapState(['isLogin', 'loginUser'])
    },
    methods: {
        logout() {
            localStorage.clear()
            this.$store.commit('SET_IS_LOGIN_STATUS', false)
            this.$store.commit('SET_USER', {})
            this.$store.commit('SET_MY_QUESTIONS', [])
            this.$store.commit('SET_QUESTIONS_BY_TAG', [])
            this.$store.commit('SET_WATCHED_TAGS', [])
            this.$store.commit('SET_WATCHED_TAGS_QUESTION', [])
            this.$alertify.message(`Logout, See you around :)`);
        }
    },
}
</script>

<style>
.nav-container {
    display: flex;
}
.nav-container > span {
    align-self: center;
}
.dropdown-menu {
    padding: 0.4em 1em;
}
.dropdown-menu:hover {
    cursor: pointer;
}
</style>
