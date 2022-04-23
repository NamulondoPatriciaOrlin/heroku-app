<template>
<v-container>
    <v-alert text v-model="alert.show" :type="alert.type" dismissible>{{alert.message}}</v-alert>
    <v-row justify="center">
        <v-col class="text-center" md="6" sm="6">
            <v-dialog v-model="dialog" persistent max-width="500px">
                <template v-slot:activator="{ on, attrs }">
                    <v-btn class="primary" dark v-bind="attrs" v-on="on" @click.native.stop="dialog = true">
                        SignUp
                    </v-btn>
                </template>
                <v-card>
                    <v-card-title>
                        <span class="text-h5">SignUp</span>
                    </v-card-title>
                    <v-form class="ma-3" @submit.prevent="signup()" ref="signupform">
                        <v-container>
                            <v-row>
                                <v-col cols="6">
                                    <v-text-field label="First name" type="name" :rules="nameRules" v-model="user.first_name"></v-text-field>
                                </v-col>
                                <v-col cols="6">
                                    <v-text-field label="Last name" type="name" :rules="nameRules" v-model="user.last_name"></v-text-field>
                                </v-col>
                                <v-col cols="12">
                                    <v-text-field label="Email" type="email" :rules="emailRules" v-model="user.email"></v-text-field>
                                </v-col>
                                <v-col cols="12">
                                    <v-text-field label="Password" type="password" :rules="passwordRules" v-model="user.password"></v-text-field>
                                </v-col>
                            </v-row>
                        </v-container>
                        <v-radio-group row v-model="user.role" :rules="[(v) => !!v || 'Please choose one']">
                            <v-radio label="Professor" value="professor"></v-radio>
                            <v-radio label="Student" value="student"></v-radio>
                        </v-radio-group>
                        <v-card-actions>
                            <v-btn color="blue darken-1" text @click="dialog = false">
                                Cancel
                            </v-btn>
                            <v-spacer></v-spacer>
                            <v-btn color="primary" type="submit">Sign up</v-btn>
                        </v-card-actions>
                    </v-form>
                </v-card>
            </v-dialog>
        </v-col>
        <!-- <v-col class="text-center" md="6" sm="6">
            <v-dialog v-model="newdialog" persistent max-width="480px">
                <template v-slot:activator="{ on, attrs }">
                    <v-btn class="secondary" dark v-bind="attrs" v-on="on" @click.native.stop="newdialog = true">
                        Login
                    </v-btn>
                </template>
                <v-card>
                    <v-card-title>
                        <span class="text-h5">Login</span>
                    </v-card-title>
                    <v-form class="ma-3" @submit.prevent="login()" ref="loginform">
                        <v-container>
                            <v-row>
                                <v-col cols="12">
                                    <v-text-field label="Email" type="email" :rules="emailRules" v-model="user.email"></v-text-field>
                                </v-col>
                                <v-col cols="12">
                                    <v-text-field label="Password" type="password" :rules="passwordRules" v-model="user.password"></v-text-field>
                                </v-col>
                            </v-row>
                        </v-container>
                        <v-radio-group row v-model="user.role" :rules="[(v) => !!v || 'Please choose one']">
                            <v-radio label="Professor" value="professor"></v-radio>
                            <v-radio label="Student" value="student"></v-radio>
                        </v-radio-group>
                        <v-card-actions>
                            <v-btn class="success" text @click="newdialog = false">
                                Cancel
                            </v-btn>
                            <v-spacer></v-spacer>
                            <v-btn color="primary" type="submit">Login</v-btn>
                        </v-card-actions>
                    </v-form>
                </v-card>
            </v-dialog>
        </v-col> -->
    </v-row>
</v-container>
</template>
 <script>
import axios from 'axios';

export default {
    data() {
        return {
            dialog: false,
            value: 1,
            newdialog: false,
            alert: {
                show: false,
                message: ''
            },
            user: {
                first_name: '',
                last_name: '',
                email: '',
                password: '',
                role: '',
            },
            nameRules: [
                value => !!value || 'Name is required',
                value => value.length <= 10 || 'Name must be less than 10 characters',
            ],
            emailRules: [
                value => {
                    const pattern = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
                    return pattern.test(value) || 'Invalid email.'
                }
            ],
            passwordRules: [
                value => !!value || 'The password is required.',
            ],
        }

    },
    methods: {
        async signup() {
            let valid = this.$refs.signupform.validate();
            if (valid) {
                try {
                    const res = await this.axios.post('/signup', this.user);
                    this.$refs.signupform.reset();
                    console.log(res);

                    this.alert = {
                        show: true,
                        type: 'success',
                        message: ''
                    };

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
}
const app = document.createElement('div');
app.setAttribute('data-app', true);
document.body.append(app);
</script>
