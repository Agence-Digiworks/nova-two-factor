<template>
  <LoadingView :loading="loading">
    <heading class="mb-6">{{ __("Two factor auth (Google 2FA)") }}</heading>
    <LoadingCard :loading="loading">
      <div class="tw-grid tw-grid-cols-2 tw-gap-4 p-4">
        <div class="">
          <div>
            <p class="mb-4 text-slate-900 dark:text-slate-400">
              {{ __("Update your two factor security settings") }}
            </p>

            <div class="tw-flex tw-items-center tw-mb-4">
              <input
                v-model="status"
                :value="true"
                id="op-enable"
                type="radio"
                class="tw-w-4 tw-h-4 tw-border-gray-300 tw-focus:ring-2 tw-focus:ring-blue-300 tw-cursor-pointer"
              />
              <label
                for="op-enable"
                class="tw-block tw-ml-2 tw-text-sm tw-font-medium dark:text-white tw-cursor-pointer"
              >
                {{ __("Enable") }}
              </label>
            </div>

            <div class="tw-flex tw-items-center tw-mb-4">
              <input
                v-model="status"
                :value="false"
                id="op-disable"
                type="radio"
                class="tw-w-4 tw-h-4 tw-border-gray-300 tw-focus:ring-2 tw-focus:ring-blue-300 tw-cursor-pointer"
              />
              <label
                for="op-disable"
                class="tw-block tw-ml-2 tw-text-sm tw-font-medium dark:text-white tw-cursor-pointer"
              >
                {{ __("Disable") }}
              </label>
            </div>

            <br />

            <DefaultButton @click="toggle"
              class="shrink-0 h-9 px-4 focus:outline-none ring-primary-200 dark:ring-gray-600 focus:ring text-white dark:text-gray-800 inline-flex items-center font-bold shadow rounded focus:outline-none ring-primary-200 dark:ring-gray-600 focus:ring bg-primary-500 hover:bg-primary-400 active:bg-primary-600 text-white dark:text-gray-800 inline-flex items-center font-bold px-4 h-9 text-sm tw-cursor-pointer"
            >{{
              __("Update Settings")
            }}</DefaultButton>
            <Link
              class="ml-3"
              as="button"
              :href="resolveNovaPath('/nova-two-factor/clear')"
            >
              {{ __("Clear settings") }}
            </Link>
          </div>
        </div>
      </div>
    </LoadingCard>
  </LoadingView>
</template>

<script>
export default {
  props: ["enabled"],
  data() {
    return {
      loading: false,
      status: this.enabled,
    };
  },

  mounted() {},

  methods: {
    toggle() {
      Nova.request()
        .post("/nova-vendor/nova-two-factor/toggle", {
          status: this.status,
        })
        .then((res) => {
          Nova.success(res.data.message);
        });
    },

    resolveNovaPath(path) {
      return Nova.url(path);
    },
  },

  computed: {
    //
  },
};
</script>
