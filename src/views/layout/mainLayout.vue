<template>
    <div>
        <App-Header/>
        <router-view/>
    </div>
</template>

<script>
import AppHeader from '@/components/appHeader.vue'
export default {
    components:{
        AppHeader: AppHeader
    }
}
</script>