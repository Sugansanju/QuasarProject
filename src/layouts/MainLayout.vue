<template>
<q-layout view="lHh Lpr lFf" class="styl">
    <q-header class="bg-white text-primary">
        <q-toolbar>
            <q-toolbar-title class="text-right">
                <q-btn round dense unelevated="" icon="menu" :ripple="false" class="q-ma-sm bg-grey-3 text-black" @click="leftDrawerOpen = !leftDrawerOpen" />
            </q-toolbar-title>
        </q-toolbar>
    </q-header>
    <q-drawer v-model="leftDrawerOpen" show-if-above="" bordered class="bg-green-6 q-pa-md " :width="400" :breakpoint="600">
        <div class="row flex justify-between">
            <div>
                <q-avatar>
                    <img src="~assets/logo.png" />
                </q-avatar>
            </div>
            <div class="social-icon q-mt-md">
                <i class="fab fa-facebook q-ml-md" aria-hidden="true"></i>
                <i class="fab fa-twitter q-ml-md" aria-hidden="true"></i>
                <i class="fab fa-google q-ml-md" aria-hidden="true"></i>
                <i class="fab fa-linkedin q-ml-md q-mr-md" aria-hidden="true"></i>

            </div>
        </div>
        <div class="container text-center q-mt-xl">
            <img class="q-mt-lg" src="~/assets/test.png" width="250" height="230" />
            <div>
                <span class="text-styl">We are Hiring</span>
            </div>
            <div class="q-pl-lg q-pr-lg para-styl">
                Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s.
            </div>
            <div class="q-mt-xl">
                <q-btn no-caps rounded color="amber" text-color="black" label="Purchase this template" style="letter-spacing:.05em;" />
            </div>
            <div class="absolute-bottom">
                <span class="copyright-styl">© 2021 sample</span>
            </div>
        </div>

    </q-drawer>

    <q-page-container>
        <router-view />
    </q-page-container>
</q-layout>
</template>

<script>

import {  ref } from 'vue'

export default ({
    name: 'MainLayout',

    components: {
    },

    setup() {
        const leftDrawerOpen = ref(false)
        return {
            leftDrawerOpen,
            toggleLeftDrawer() {
                leftDrawerOpen.value = !leftDrawerOpen.value
            }
        }
    }
})
</script>

<style scoped>
.styl {
   font-family: ui-monospace;

}

.social-icon {
    color: #efefef;
}

.text-styl {
    font-size: 2.3em;
    color: white;
    letter-spacing: .07em;
    font-weight: bold;
}

.para-styl {
    font-size: 1.1em;
    color: white;
}

.copyright-styl {
    font-size: 1em;
    color: #efefef;
}
</style>
