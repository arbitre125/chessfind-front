<template>
    <footer>
        <div class="content-container">
            {{ new Date().getFullYear() }} {{ $t('footer.copyright') }}
        </div>
    </footer>
</template>

<script>
export default {}
</script>

<style scoped>
.content-container {
    padding: 2px 0;
    display: flex;
    width: 92%;
    margin: 0 4%;
}
</style>
