<template>
    <LoadingView :loading="false">
        <heading class="mb-6">{{ __('Two factor auth (Google 2FA)') }}</heading>

        <Card>
            <div class="p-4">
                <p class="mb-3">{{ __(`By clearing these settings you can re-register Two FA on your device`) }}
                </p>
                <form @submit.prevent="auth()" class="max-w-[25rem]">
                    <div class="mb-6">
                        <label class="block mb-2" for="password">{{ __('Enter your password') }}</label>
                        <input v-model="form.password" class="form-control form-input form-input-bordered w-full"
                            id="password" type="password" name="password" ref="password" autofocus="" required />

                        <HelpText class="mt-2 text-red-500" v-if="form.errors.has('password')">
                            {{ form.errors.first('password') }}
                        </HelpText>
                    </div>

                    <DefaultButton 
                    class="tw-inline-flex tw-items-center tw-bg-primary-500 tw-border tw-border-transparent tw-text-white tw-text-sm tw-font-semibold tw-rounded tw-px-4 tw-py-2 tw-transition tw-duration-150 tw-ease-in-out hover:tw-bg-primary-600 disabled:tw-opacity-50 disabled:tw-cursor-not-allowed" type="submit">
                        <span>
                            {{ __('Clear') }}
                        </span>
                    </DefaultButton>

                </form>
            </div>

        </Card>
    </LoadingView>
</template>

<script>
export default {
    data() {
        return {
            form: Nova.form({
                password: null
            }),
        }
    },

    mounted() {
        this.$nextTick(() => this.$refs.password.focus())
    },

    methods: {
        async auth() {

            const { message } = await this.form.post('/nova-vendor/nova-two-factor/clear')

            Nova.success(message)

            Nova.visit('/nova-two-factor')
        },
    }
}
</script>
