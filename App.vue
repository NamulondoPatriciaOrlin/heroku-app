<template>
<v-app>
    <v-main>
        <TopNavigationBar />
        <router-view></router-view>
        <!-- <Footer />      -->
    </v-main>
</v-app>
</template>

<script>
import TopNavigationBar from './components/signup'
export default {
    name: "App",
    components: {
        TopNavigationBar,
        // Footer
    },
    data: () => ({})
};
</script>
  methods: {
        async signup() {
            let valid = this.$refs.signupform.validate();
            if (valid) {
                try {
                    const res = await this.axios.post('./signup', this.user);
                    this.$refs.signupform.reset();
                    console.log(res);
                    this.alert = {
                        show: true,
                        type: 'success',
                        message: ''
                    }
                } catch (error) {
                    this.alert = {
                        show: true,
                        type: 'error',
                        message: ''
                    }
                }
            }
        }
    }
   