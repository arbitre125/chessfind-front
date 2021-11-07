<template>
    <div class="navbar">
        <div class="content-container">
            <Logo class="navbar-logo" />
            <div class="navbar-right">
                <input
                    v-model="searchInput"
                    type="text"
                    placeholder="Search..."
                    class="navbar-search"
                />
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            searchInput: ''
        }
    },
    watch: {
        searchInput(newValue, oldValue) {
            if (newValue !== oldValue) this.$emit('newInputValue', newValue)
        }
    }
}
</script>

<style scoped>
.navbar {
    width: 100%;
    background-color: var(--color-white);
    border-bottom: 1px solid var(--color-border);
    overflow: hidden;
    position: fixed;
    top: 0;
}

.content-container {
    padding: 2px 20px;
    display: flex;
}

.navbar-logo {
    width: 120px;
    height: 40px;
    margin-right: 40px;
}

.navbar-right {
    flex-grow: 1;
    margin: 4px 0;
    display: flex;
}

.navbar-total {
    width: 140px;
    display: flex;
    flex-direction: column;
    justify-content: center;
}

.navbar-search {
    flex-grow: 1;
    padding: 0 12px;
    border-radius: 8px;
    border: 1px solid var(--color-border);
}
</style>
