<template>
    <div class="navbar">
        <div class="content-container">
            <Logo class="navbar-logo" />
            <div class="navbar-right">
                <input
                    v-model="searchInput"
                    type="text"
                    :placeholder="$t('action.search') + '...'"
                    class="navbar-search"
                />
                <button
                    class="navbar-filter"
                    @click="displayFilter = !displayFilter"
                >
                    <span v-if="displayFilter">{{ $t('action.close') }}</span>
                    <span v-else>{{ $t('action.filter') }}</span>
                </button>
            </div>
        </div>
        <div v-if="displayFilter" class="content-container">
            <div class="tournaments-filter">
                <div class="filter-item">
                    <label>
                        {{ $t('filter.min_date') }}
                        <input v-model="minDate" type="date" />
                    </label>
                </div>
                <div class="filter-item">
                    <label>
                        {{ $t('filter.max_date') }}
                        <input v-model="maxDate" type="date" :min="minDate" />
                    </label>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            searchInput: '',
            displayFilter: false,
            minDate: '',
            maxDate: ''
        }
    },
    watch: {
        searchInput(newValue, oldValue) {
            if (newValue !== oldValue) {
                this.$emit('newInputValue', newValue)
            }
        },
        minDate(newValue, oldValue) {
            if (newValue !== oldValue) {
                this.$emit('newMinDate', newValue)
            }
        },
        maxDate(newValue, oldValue) {
            if (newValue !== oldValue) {
                this.$emit('newMaxDate', newValue)
            }
        }
    }
}
</script>

<style scoped>
.navbar {
    width: 100%;
    background-color: var(--color-white);
    box-shadow: var(--color-shadow);
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
    min-width: 40px;
    height: 40px;
    margin-right: 1rem;
    padding: 4px 0;
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

.navbar-filter {
    margin-left: 12px;
    background-color: var(--color-white);
    border: 1px solid var(--color-border);
    padding: 4px 12px;
    border-radius: 8px;
    cursor: pointer;
    width: 60px;
}
.navbar-filter:hover {
    border-color: var(--color-border-hover);
}

.filter-header {
    margin: 8px 0;
    font-size: 1.2rem;
}
.tournaments-filter {
    width: 100%;
    margin-bottom: 12px;
}
input[type='checkbox'],
input[type='radio'] {
    vertical-align: middle;
    position: relative;
    bottom: 1px;
}
.filter-item {
    display: inline-block;
    margin: 4px 40px 4px 0;
}
</style>
