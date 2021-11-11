<template>
    <div class="navbar">
        <div class="content-container">
            <Logo class="navbar-logo" />
            <div class="navbar-right mobile">
                <div class="icon-menu" @click="displayFilter = !displayFilter">
                    <IconClose v-if="displayFilter" />
                    <IconMenu v-else />
                </div>
            </div>
            <div class="navbar-right desktop">
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
                    <div v-if="displayFilter" class="close-filter">
                        <span>{{ $t('action.close') }}</span>
                    </div>
                    <div v-else class="open-filter">
                        <span>{{ $t('action.filter') }}</span>
                    </div>
                </button>
            </div>
        </div>
        <div v-if="displayFilter" class="content-container">
            <div class="tournaments-filter">
                <input
                    v-model="searchInput"
                    type="text"
                    :placeholder="$t('action.search') + '...'"
                    class="search-menu mobile"
                />
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
                <div class="filter-item">
                    <label>
                        {{ $t('filter.region') }}
                        <select v-model="region">
                            <option value="">
                                {{ $t('action.select_region') }}
                            </option>
                            <option
                                v-for="reg in filterRegions"
                                :key="reg[1]"
                                :value="reg[1]"
                            >
                                {{ reg[0] }}
                            </option>
                        </select>
                    </label>
                </div>
                <div class="filter-item clean">
                    <button
                        class="navbar-filter clean"
                        :disabled="emptyFilters"
                        @click="cleanFilters"
                    >
                        {{ $t('action.clean_filters') }}
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { getCountries } from '../utils/countries'
import IconClose from './icons/IconClose'
import IconMenu from './icons/IconMenu'

export default {
    components: {
        IconClose,
        IconMenu
    },
    data() {
        return {
            searchInput: '',
            displayFilter: true,
            minDate: '',
            maxDate: '',
            region: ''
        }
    },
    computed: {
        filterRegions() {
            const countries = getCountries().map((name) => [
                this.$t(`region.${name.toLowerCase()}`),
                name
            ])
            return new Map([...countries].sort())
        },
        emptyFilters() {
            return (
                this.minDate === '' && this.maxDate === '' && this.region === ''
            )
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
        },
        region(newValue, oldValue) {
            if (newValue !== oldValue) {
                this.$emit('newRegion', newValue)
            }
        }
    },
    methods: {
        cleanFilters() {
            this.minDate = ''
            this.maxDate = ''
            this.region = ''
            this.$emit('cleanFilters')
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
    position: sticky;
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

.navbar-right.mobile {
    flex-direction: row;
    justify-content: flex-end;
    align-items: center;
}

.navbar-total {
    width: 140px;
    display: flex;
    flex-direction: column;
    justify-content: center;
}

.navbar-search {
    flex-grow: 1;
    padding-left: 12px;
}

.navbar-filter {
    margin-left: 12px;
    background-color: var(--color-white);
    border: 1px solid var(--color-border);
    padding: 4px 12px;
    border-radius: var(--border-radius);
    cursor: pointer;
    width: 80px;
}
.navbar-filter:hover {
    border-color: var(--color-border-hover);
}

.navbar-filter.clean {
    margin: 0;
    width: 100%;
    height: 32px;
}

.open-filter,
.close-filter {
    display: inline-flex;
    align-content: center;
    align-items: flex-end;
}

.filter-header {
    margin: 8px 0;
    font-size: 1.2rem;
}
.tournaments-filter {
    width: 100%;
    margin-bottom: 12px;
}

.filter-item {
    display: inline-block;
    margin: 8px 40px 8px 0;
    color: var(--color-black-light);
    width: 200px;
}

.filter-item input,
.filter-item select {
    display: block;
    margin-top: 4px;
    width: 100%;
    cursor: pointer;
}

.filter-item input:hover,
.filter-item select:hover {
    border-color: var(--color-border-hover);
}

.filter-item.clean {
    margin: 4px 0 0 0;
    width: 120px;
}

@media only screen and (max-width: 768px) {
    .filter-item.clean {
        margin-top: 12px;
    }
}

.icon-menu {
    width: 24px;
    height: 24px;
    margin-right: 4px;
    cursor: pointer;
}

.search-menu {
    width: 100%;
    margin: 4px 0 8px 0;
}
</style>
