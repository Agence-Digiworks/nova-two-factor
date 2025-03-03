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
            class="tw-inline-flex tw-items-center tw-bg-primary-500 tw-border tw-border-transparent tw-text-white tw-text-sm tw-font-semibold tw-rounded tw-px-4 tw-py-2 tw-transition tw-duration-150 tw-ease-in-out hover:tw-bg-primary-600 disabled:tw-opacity-50 disabled:tw-cursor-not-allowed" type="submit">
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
