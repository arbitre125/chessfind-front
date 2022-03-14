<template>
    <div class="skeleton-sections">
        <div class="skeleton-header"></div>
        <div class="skeleton-body"></div>
        <div class="skeleton-body"></div>

        <div class="skeleton-header"></div>
        <div class="skeleton-body"></div>

        <div class="skeleton-header"></div>
        <div class="skeleton-body"></div>
    </div>
</template>

<style scoped>
.skeleton-header {
    height: 28px;
    margin-top: 40px;
    margin-bottom: 8px;
    width: 100%;
}

.skeleton-body {
    height: 80px;
    margin-bottom: 16px;
}

.skeleton-header,
.skeleton-body {
    background-color: #94a3b8;
    animation: pulse 2s infinite ease-in-out;
}
</style>
