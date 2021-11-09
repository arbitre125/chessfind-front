<template>
    <div class="skeleton-card">
        <div class="skeleton-header"></div>
        <div class="skeleton-body"></div>
    </div>
</template>

<style scoped>
.skeleton-card {
    margin: 8px 0;
    padding: 12px 16px 16px 16px;
    border-radius: var(--border-radius);
    background-color: #e2e8f0;
    height: 87px;
}

.skeleton-header {
    height: 28px;
    margin-bottom: 8px;
}

.skeleton-body {
    height: 21px;
}

.skeleton-header,
.skeleton-body {
    background-color: #94a3b8;
    animation: pulse 2s infinite ease-in-out;
}

@keyframes pulse {
    0% {
        background-color: #94a3b8;
    }

    50% {
        background-color: #cbd5e1;
    }

    100% {
        background-color: #94a3b8;
    }
}
</style>
