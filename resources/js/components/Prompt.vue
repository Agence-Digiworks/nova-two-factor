<template>
    <div>
        <form @submit.prevent="auth()" class="bg-white dark:bg-gray-800 shadow rounded-lg p-8 max-w-[25rem] mx-auto">
            <h2 class="text-2xl text-center font-normal mb-6 text-90">
                {{ __('Two factor authentication') }}
            </h2>

            <DividerLine />

            <div class="mb-6">
                <label class="block mb-2" for="otp">{{ __('One time password') }}</label>
                <input v-model="otp" @keyup="autoSubmit()" class="form-control form-input form-input-bordered w-full"
                    maxLength="6" id="otp" type="text" name="otp" ref="otp" autofocus="" required />

                <HelpText class="mt-2 text-red-500" v-if="error">
                    {{ error }}
                </HelpText>
            </div>

            <DefaultButton 
            class="shrink-0 h-9 px-4 focus:outline-none ring-primary-200 dark:ring-gray-600 focus:ring text-white dark:text-gray-800 inline-flex items-center font-bold shadow rounded focus:outline-none ring-primary-200 dark:ring-gray-600 focus:ring bg-primary-500 hover:bg-primary-400 active:bg-primary-600 text-white dark:text-gray-800 inline-flex items-center font-bold px-4 h-9 text-sm tw-cursor-pointer" type="submit">
                <span>
                    {{ __('Authenticate') }}
                </span>
            </DefaultButton>

        </form>
    </div>
</template>

<script>
export default {
    props: ['referer'],
    data() {
        return {
            otp: '',
            error: null
        }
    },

    mounted() {
        this.$nextTick(() => this.$refs.otp.focus())
    },

    methods: {
        auth() {
            let self = this
            Nova.request().post('/nova-vendor/nova-two-factor/validatePrompt', {
                one_time_password: this.otp
            })
                .then(res => {
                    window.location.href = self.referer
                })
                .catch(e => {
                    this.error = e.response.data.message
                })
        },

        autoSubmit() {
            if (this.otp.length === 6) {
                this.auth()
            }
        }
    }
}
</script>
