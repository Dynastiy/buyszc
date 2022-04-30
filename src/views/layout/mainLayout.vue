<template>
    <div>
        <App-Header/>
        <router-view/>
        <App-Footer/>
    </div>
</template>

<script>
import AppHeader from '@/components/appHeader.vue'
import AppFooter from '@/components/static/appFooter.vue'
export default {
    components:{
        AppHeader: AppHeader,
        AppFooter: AppFooter
    }
}
</script>