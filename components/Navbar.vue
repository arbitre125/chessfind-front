<template>
    <div class="navbar">
        <div class="content-container">
            <Logo class="navbar-logo" />
            <div class="navbar-right mobile">
                <div class="icon-menu" @click="toggleMovileMenu">
                    <IconClose v-if="displayMobileMenu" />
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
        <div
            v-if="displayFilter || displayMobileMenu"
            class="content-container"
        >
            <div class="tournaments-filter">
                <input
                    v-if="displayMobileMenu"
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
                            <option
                                v-for="reg in filterRegions"
                                :key="reg"
                                :value="reg"
                            >
                                {{ $t(`region.${reg.toLowerCase()}`) }}
                            </option>
                        </select>
                    </label>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import IconClose from './icons/IconClose'
import IconMenu from './icons/IconMenu'

export default {
    components: {
        IconClose,
        IconMenu
    },
    data() {
        return {
            displayMobileMenu: false,
            searchInput: '',
            displayFilter: false,
            minDate: '',
            maxDate: '',
            region: 'ESP'
        }
    },
    computed: {
        filterRegions() {
            return [
                'ALB',
                'ALG',
                'AND',
                'ARG',
                'ARM',
                'ARU',
                'AUS',
                'AUT',
                'AZE',
                'BAN',
                'BEL',
                'BIH',
                'BLR',
                'BOL',
                'BRA',
                'BRN',
                'BUL',
                'CAN',
                'CHI',
                'CHN',
                'CIV',
                'CMR',
                'COL',
                'CRC',
                'CRO',
                'CUB',
                'CYP',
                'CZE',
                'DEN',
                'DOM',
                'ECU',
                'EGY',
                'ENG',
                'ESA',
                'ESP',
                'EST',
                'FAI',
                'FIN',
                'FRA',
                'GEO',
                'GER',
                'GHA',
                'GRE',
                'GUA',
                'HKG',
                'HON',
                'HUN',
                'INA',
                'IND',
                'IRI',
                'IRQ',
                'IRL',
                'ISL',
                'ISR',
                'ITA',
                'JAM',
                'JOR',
                'JPN',
                'KAZ',
                'KEN',
                'KGZ',
                'KOR',
                'KOS',
                'KSA',
                'KUW',
                'LAT',
                'LBA',
                'LBN',
                'LBR',
                'LES',
                'LIE',
                'LTU',
                'LUX',
                'MAD',
                'MAR',
                'MAS',
                'MDA',
                'MDV',
                'MEX',
                'MGL',
                'MKD',
                'MLT',
                'MNC',
                'MNE',
                'NAM',
                'NCA',
                'NED',
                'NEP',
                'NGR',
                'NOR',
                'PAK',
                'PAN',
                'PAR',
                'PER',
                'PHI',
                'POL',
                'POR',
                'PUR',
                'QAT',
                'ROU',
                'RSA',
                'RUS',
                'SCO',
                'SEN',
                'SGP',
                'SLO',
                'SRB',
                'SRI',
                'SSD',
                'SUD',
                'SUI',
                'SVK',
                'SWE',
                'THA',
                'TOG',
                'TTO',
                'TUN',
                'TUR',
                'UAE',
                'UGA',
                'UKR',
                'URU',
                'USA',
                'UZB',
                'VEN',
                'VIE',
                'ZAM',
                'ZIM'
            ].sort()
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
        toggleMovileMenu() {
            this.displayMobileMenu = !this.displayMobileMenu
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
