<template>
    <ion-page>
        <ion-header>
            <ion-toolbar>
                <!-- <ion-buttons slot="start">
                    <ion-back-button ></ion-back-button>
                </ion-buttons> -->
                <div class="nav-bar">
                    <ion-title class="menu-toogle-wrap" v-if="!isExpanded">
                        <ion-button fill="clear" @click="toogleMenu">
                            <ion-icon :icon="menu"></ion-icon>
                        </ion-button>
                    </ion-title>
                    <ion-title v-if="isExpanded">
                        <ion-button fill="clear" @click="toogleMenu">
                            <ion-icon :icon="arrowBack"></ion-icon>
                        </ion-button>
                    </ion-title>
                    <!-- <ion-title>Login</ion-title> -->
                </div>
            </ion-toolbar>
        </ion-header>
        <ion-content>
            <div class="dropdown-menu" v-if="isExpanded">
                <ion-list class="dropdown-menu-nav">
                    <ion-item class="dropdown-menu-item">
                        <a class="dropdown-menu-link" href="/home">
                            <div class="dropdown-menu-text">Home</div>
                        </a>
                    </ion-item>
                    <ion-item class="dropdown-menu-item">
                        <a class="dropdown-menu-link" href="/products/add">
                            <div class="dropdown-menu-text">Add Product</div>
                        </a>
                    </ion-item>
                    <ion-item class="dropdown-menu-item" v-if="!getLoginStatus">
                        <a class="dropdown-menu-link" href="/login">
                            <div class="dropdown-menu-text">Login</div>
                        </a>
                    </ion-item>
                    <ion-item class="dropdown-menu-item" v-if="getLoginStatus" @click="logOut">
                        <a class="dropdown-menu-link">
                            <div class="dropdown-menu-text">Logout</div>
                        </a>
                    </ion-item>
                </ion-list>
            </div>
            <slot />
        </ion-content>
    </ion-page>
</template>

<script lang="ts">
import {
    IonPage,
    IonHeader,
    IonToolbar,
    IonTitle,
    IonContent,
    IonIcon,
    IonButton,
    IonItem,
    IonList,

} from '@ionic/vue';
import { defineComponent } from 'vue';
import { menu, arrowBack } from 'ionicons/icons';
// import { mapGetters } from 'vuex';
import store from './../../store/index';
import { Component } from 'ionicons/dist/types/stencil-public-runtime';
import { mapGetters } from 'vuex';



export default defineComponent({
    props: ["pageDefaultBackLink"],
    components: {
        IonPage,
        IonHeader,
        IonToolbar,
        IonTitle,
        IonContent,
        IonIcon,
        IonButton,
        IonItem,
        IonList,
    },
    setup() {
        return {
            menu,
            arrowBack
        }
    },
    // state
    data() {
        return {
            isExpanded: false,
            loginStatus: false,
        }
    },
    // actions
    methods: {
        toogleMenu() {
            this.isExpanded = !this.isExpanded;
        },
        async logOut() {
            try {
                await store.dispatch("logout");
                this.$router.push('/home');
            } catch (err: any) {
                console.log(err);
            }
        }
    },
    computed: {
        ...mapGetters(['getLoginStatus'])
    }
})
</script>

<style scoped>
/* .nav-bar{
    display:flex;
} */

ion-title {
    align-items: center;
}

.menu-icon {
    justify-content: end;
}

.dropdown_menu {}

.dropdown-menu-nav {
    /* display: flex; */
    /* flex-direction: row; */
}

.dropdown-menu-item {
    /* flex: auto; */
}

.dropdown-menu-link {
    text-decoration: none;
    color: black;
    margin-top: 0.3rem;
    margin-top: 0.1rem;
}

.dropdown-menu-text {
    font-weight: 300;
    line-height: 25px;
}
</style>