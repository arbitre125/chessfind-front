<template>
    <footer>
        <div class="content-container">
            {{ new Date().getFullYear() }}
            {{ $t('footer.copyright') }}
            |<a
                href="https://github.com/eduayme/chessfind-front/issues/new?assignees=&labels=enhancement%2C+help+wanted&template=feature_request.md&title=%5BFEATURE%5D+Name+of+the+feature"
                target="_blank"
                >Request a feature</a
            >|<a
                href="https://github.com/eduayme/chessfind-front/issues/new?assignees=&labels=enhancement%2C+help+wanted&template=city_request.md&title=%5BCITY%5D+XXX+request"
                target="_blank"
                >Request a city</a
            >|<a
                href="https://github.com/eduayme/chessfind-front/issues/new?assignees=&labels=bug%2C+help+wanted&template=bug_report.md&title=%5BBUG%5D+Name+of+the+bug"
                target="_blank"
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
