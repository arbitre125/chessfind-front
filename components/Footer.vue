<template>
    <footer>
        <div class="content-container">
            {{ new Date().getFullYear() }}
            {{ $t('footer.copyright') }}
            |<a href="https://github.com/eduayme/chessfind-front"
                >Request a feature</a
            >|<a href="https://github.com/eduayme/chessfind-front"
                >Request a city</a
            >|<a href="https://github.com/eduayme/chessfind-front"
                >Report a bug</a
            >
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
span {
    margin-right: 40px;
}
a {
    color: var(--color-white);
    margin: 0 8px;
}
</style>
