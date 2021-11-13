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
                    class="navbar-filter display"
                    @click="displayFilter = !displayFilter"
                >
                    <div v-if="displayFilter" class="close-filter">
                        <IconClose />
                    </div>
                    <div v-else class="open-filter">
                        <IconFilter />
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
                <div class="filter-item min-date">
                    <label>
                        {{ $t('filter.min_date') }}
                        <input v-model="minDate" type="date" />
                    </label>
                </div>
                <div class="filter-item max-date">
                    <label>
                        {{ $t('filter.max_date') }}
                        <input v-model="maxDate" type="date" :min="minDate" />
                    </label>
                </div>

                <div class="filter-item regions">
                    <label>
                        {{ $t('filter.regions') }}
                        <multiselect
                            v-model="regions"
                            placeholder="Search region"
                            label="name"
                            track-by="code"
                            :options="filterCountries"
                            :multiple="true"
                            :close-on-select="true"
                        >
                            <span slot="noResult">No results found.</span>
                        </multiselect>
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
import Multiselect from 'vue-multiselect'
import { getCountries } from '../utils/filters'
import IconClose from './icons/IconClose'
import IconMenu from './icons/IconMenu'
import IconFilter from './icons/IconFilter'

export default {
    components: {
        IconClose,
        IconMenu,
        IconFilter,
        Multiselect
    },
    data() {
        return {
            searchInput: '',
            displayFilter: true,
            minDate: '',
            maxDate: '',
            regions: []
        }
    },
    computed: {
        filterRegions() {
            return [
                {
                    region: this.$t('region.sa'),
                    countries: [
                        { name: this.$t('region.arg'), code: 'ARG' },
                        { name: this.$t('region.arm'), code: 'ARM' }
                    ]
                },
                {
                    region: this.$t('region.eu'),
                    countries: [
                        { name: this.$t('region.esp'), code: 'ESP' },
                        { name: this.$t('region.ger'), code: 'GER' }
                    ]
                }
            ]
        },
        filterCountries() {
            const countries = getCountries().map((code) => ({
                name: this.$t(`region.${code.toLowerCase()}`),
                code
            }))
            return countries.sort((a, b) => (a.name > b.name ? 1 : -1))
        },
        emptyFilters() {
            return (
                this.minDate === '' &&
                this.maxDate === '' &&
                this.regions.length === 0
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
        regions(newValue, oldValue) {
            const value = newValue.map((res) => res.code)
            if (value !== oldValue) {
                this.$emit('newRegions', value)
            }
        }
    },
    methods: {
        cleanFilters() {
            this.minDate = ''
            this.maxDate = ''
            this.regions = []
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
    width: 60px;
}
.navbar-filter:hover {
    border-color: var(--color-border-hover);
}

.navbar-filter.display {
    padding: 0;
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
    width: 24px;
    padding: 2px;
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
    width: 288px;
}

.filter-item.min-date,
.filter-item.max-date {
    width: 160px;
}

.filter-item.regions {
    max-height: 32px !important;
}

.filter-item input,
.filter-item select,
.multiselect {
    display: block;
    margin-top: 4px !important;
    width: 100%;
    cursor: pointer;
    background-color: var(--color-white);
}

.filter-item input:hover,
.filter-item select:hover {
    border-color: var(--color-border-hover);
}

@media only screen and (max-width: 524px) {
    .filter-item,
    .filter-item.clean {
        margin-right: 0;
        width: 100% !important;
    }
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
