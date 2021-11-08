<template>
    <div class="default-container">
        <div class="error-info title">
            <h2 v-if="error.statusCode === 404">
                {{ $t('error.page_not_found') }}
            </h2>
            <h2 v-else>
                {{ $t('error.error_found') }}
            </h2>
        </div>
        <div class="error-actions">
            <NuxtLink to="/">
                {{ $t('home') }}
            </NuxtLink>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Home',
    layout: 'error',
    // eslint-disable-next-line vue/require-prop-types
    props: ['error'],
    head() {
        return {
            title: this.$t('error.error-found')
        }
    }
}
</script>

<style scoped>
.error-info {
    margin-bottom: 40px;
}
</style>
