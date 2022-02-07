<template>
    <div class="navbar">
        <div class="content-container">
            <Logo class="navbar-logo" />
            <div class="navbar-right mobile">
                <div class="icon-menu" @click="displayFilter = !displayFilter">
                    <IconClose v-if="displayFilter" />
                    <div v-else>
                        <IconMenu />
                        <div v-if="!emptyFilters" class="active-filter"></div>
                    </div>
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
                        <div v-if="!emptyFilters" class="active-filter"></div>
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
                    :disabled="loading"
                />

                <div class="filter-item regions">
                    <label>
                        {{ $t('filter.regions') }}
                        <multiselect
                            v-model="regions"
                            placeholder="Search region"
                            label="name"
                            track-by="code"
                            group-label="region"
                            group-values="countries"
                            :group-select="true"
                            :options="filterRegions"
                            :multiple="true"
                            :close-on-select="true"
                            :hide-selected="true"
                            :disabled="loading"
                        >
                            <span slot="noResult">No results found.</span>
                        </multiselect>
                    </label>
                </div>

                <div class="filter-item min-date">
                    <label>
                        {{ $t('filter.min_date') }}
                        <input
                            v-model="minDate"
                            type="date"
                            :min="today"
                            :disabled="loading"
                        />
                    </label>
                </div>
                <div class="filter-item max-date">
                    <label>
                        {{ $t('filter.max_date') }}
                        <input
                            v-model="maxDate"
                            type="date"
                            :min="minDate || today"
                            :disabled="loading"
                        />
                    </label>
                </div>
                <div class="filter-item min-date">
                    <label>
                        {{ $t('filter.min_days') }}
                        <input
                            v-model="minDays"
                            type="number"
                            min="1"
                            :disabled="loading"
                        />
                    </label>
                </div>
                <div class="filter-item max-date">
                    <label>
                        {{ $t('filter.max_days') }}
                        <input
                            v-model="maxDays"
                            type="number"
                            :min="minDays || 1"
                            :disabled="loading"
                        />
                    </label>
                </div>

                <div class="filter-item clean">
                    <button
                        class="navbar-filter clean"
                        :disabled="emptyFilters || loading"
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
    props: {
        loading: {
            type: Boolean,
            default: false
        }
    },
    data() {
        return {
            searchInput: '',
            displayFilter: false,
            minDate: '',
            maxDate: '',
            minDays: '',
            maxDays: '',
            regions: []
        }
    },
    computed: {
        today() {
            return new Date().toISOString().split('T')[0]
        },
        filterRegions() {
            return [
                {
                    region: this.$t('region.af'),
                    countries: [
                        { name: this.$t('region.alg'), code: 'ALG' },
                        { name: this.$t('region.cmr'), code: 'CMR' },
                        { name: this.$t('region.egy'), code: 'EGY' },
                        { name: this.$t('region.gha'), code: 'GHA' },
                        { name: this.$t('region.civ'), code: 'CIV' },
                        { name: this.$t('region.ken'), code: 'KEN' },
                        { name: this.$t('region.les'), code: 'LES' },
                        { name: this.$t('region.lbr'), code: 'LBR' },
                        { name: this.$t('region.lba'), code: 'LBA' },
                        { name: this.$t('region.mad'), code: 'MAD' },
                        { name: this.$t('region.mar'), code: 'MAR' },
                        { name: this.$t('region.nam'), code: 'NAM' },
                        { name: this.$t('region.ngr'), code: 'NIG' },
                        { name: this.$t('region.sen'), code: 'SEN' },
                        { name: this.$t('region.rsa'), code: 'RSA' },
                        { name: this.$t('region.ssd'), code: 'SSD' },
                        { name: this.$t('region.sud'), code: 'SUD' },
                        { name: this.$t('region.tog'), code: 'TOG' },
                        { name: this.$t('region.tun'), code: 'TUN' },
                        { name: this.$t('region.uga'), code: 'UGA' },
                        { name: this.$t('region.zam'), code: 'ZAM' },
                        { name: this.$t('region.zim'), code: 'ZIM' }
                    ]
                },
                {
                    region: this.$t('region.as'),
                    countries: [
                        { name: this.$t('region.aze'), code: 'AZE' },
                        { name: this.$t('region.ban'), code: 'BAN' },
                        { name: this.$t('region.brn'), code: 'BRN' },
                        { name: this.$t('region.chn'), code: 'CHN' },
                        { name: this.$t('region.egy'), code: 'EGY' },
                        { name: this.$t('region.geo'), code: 'GEO' },
                        { name: this.$t('region.hkg'), code: 'HKG' },
                        { name: this.$t('region.ind'), code: 'IND' },
                        { name: this.$t('region.ina'), code: 'INA' },
                        { name: this.$t('region.iri'), code: 'IRA' },
                        { name: this.$t('region.irq'), code: 'IRQ' },
                        { name: this.$t('region.isr'), code: 'ISR' },
                        { name: this.$t('region.jpn'), code: 'JPN' },
                        { name: this.$t('region.jor'), code: 'JOR' },
                        { name: this.$t('region.kaz'), code: 'KAZ' },
                        { name: this.$t('region.kuw'), code: 'KUW' },
                        { name: this.$t('region.kgz'), code: 'KGZ' },
                        { name: this.$t('region.lbn'), code: 'LBN' },
                        { name: this.$t('region.mas'), code: 'MAS' },
                        { name: this.$t('region.mdv'), code: 'MDV' },
                        { name: this.$t('region.mgl'), code: 'MGL' },
                        { name: this.$t('region.nep'), code: 'NEP' },
                        { name: this.$t('region.pak'), code: 'PAK' },
                        { name: this.$t('region.phi'), code: 'PHI' },
                        { name: this.$t('region.qat'), code: 'QAT' },
                        { name: this.$t('region.rus'), code: 'RUS' },
                        { name: this.$t('region.ksa'), code: 'KSA' },
                        { name: this.$t('region.sgp'), code: 'SGP' },
                        { name: this.$t('region.kor'), code: 'KOR' },
                        { name: this.$t('region.sri'), code: 'SRI' },
                        { name: this.$t('region.tha'), code: 'THA' },
                        { name: this.$t('region.tur'), code: 'TUR' },
                        { name: this.$t('region.uae'), code: 'UAE' },
                        { name: this.$t('region.uzb'), code: 'UZB' },
                        { name: this.$t('region.vie'), code: 'VIE' }
                    ]
                },
                {
                    region: this.$t('region.eu'),
                    countries: [
                        { name: this.$t('region.alb'), code: 'ALB' },
                        { name: this.$t('region.and'), code: 'AND' },
                        { name: this.$t('region.arm'), code: 'ARM' },
                        { name: this.$t('region.aus'), code: 'AUS' },
                        { name: this.$t('region.aze'), code: 'AZE' },
                        { name: this.$t('region.bel'), code: 'BEL' },
                        { name: this.$t('region.blr'), code: 'BLR' },
                        { name: this.$t('region.bih'), code: 'BIH' },
                        { name: this.$t('region.bul'), code: 'BUL' },
                        { name: this.$t('region.cro'), code: 'CRO' },
                        { name: this.$t('region.cyp'), code: 'CYP' },
                        { name: this.$t('region.cze'), code: 'CZE' },
                        { name: this.$t('region.den'), code: 'DEN' },
                        { name: this.$t('region.eng'), code: 'ENG' },
                        { name: this.$t('region.est'), code: 'EST' },
                        { name: this.$t('region.fai'), code: 'FAI' },
                        { name: this.$t('region.fin'), code: 'FIN' },
                        { name: this.$t('region.fra'), code: 'FRA' },
                        { name: this.$t('region.geo'), code: 'GEO' },
                        { name: this.$t('region.ger'), code: 'GER' },
                        { name: this.$t('region.gre'), code: 'GRE' },
                        { name: this.$t('region.hun'), code: 'HUN' },
                        { name: this.$t('region.isl'), code: 'ISL' },
                        { name: this.$t('region.irl'), code: 'IRL' },
                        { name: this.$t('region.ita'), code: 'ITA' },
                        { name: this.$t('region.kaz'), code: 'KAZ' },
                        { name: this.$t('region.kos'), code: 'KOS' },
                        { name: this.$t('region.lat'), code: 'LAT' },
                        { name: this.$t('region.lie'), code: 'LIE' },
                        { name: this.$t('region.ltu'), code: 'LTU' },
                        { name: this.$t('region.lux'), code: 'LUX' },
                        { name: this.$t('region.mlt'), code: 'MLT' },
                        { name: this.$t('region.mda'), code: 'MDA' },
                        { name: this.$t('region.mnc'), code: 'MNC' },
                        { name: this.$t('region.mne'), code: 'MNE' },
                        { name: this.$t('region.ned'), code: 'NED' },
                        { name: this.$t('region.mkd'), code: 'MKD' },
                        { name: this.$t('region.nor'), code: 'NOR' },
                        { name: this.$t('region.pol'), code: 'POL' },
                        { name: this.$t('region.por'), code: 'POR' },
                        { name: this.$t('region.rou'), code: 'ROU' },
                        { name: this.$t('region.rus'), code: 'RUS' },
                        { name: this.$t('region.sco'), code: 'SCO' },
                        { name: this.$t('region.srb'), code: 'SRB' },
                        { name: this.$t('region.svk'), code: 'SVK' },
                        { name: this.$t('region.slo'), code: 'SLO' },
                        { name: this.$t('region.esp'), code: 'ESP' },
                        { name: this.$t('region.swe'), code: 'SWE' },
                        { name: this.$t('region.sui'), code: 'SUI' },
                        { name: this.$t('region.tur'), code: 'TUR' },
                        { name: this.$t('region.ukr'), code: 'UKR' }
                    ]
                },
                {
                    region: this.$t('region.na'),
                    countries: [
                        { name: this.$t('region.can'), code: 'CAN' },
                        { name: this.$t('region.crc'), code: 'CRC' },
                        { name: this.$t('region.cub'), code: 'CUB' },
                        { name: this.$t('region.dom'), code: 'DOM' },
                        { name: this.$t('region.esa'), code: 'ESA' },
                        { name: this.$t('region.gua'), code: 'GUA' },
                        { name: this.$t('region.hon'), code: 'HON' },
                        { name: this.$t('region.jam'), code: 'JAM' },
                        { name: this.$t('region.mex'), code: 'MEX' },
                        { name: this.$t('region.nca'), code: 'NCA' },
                        { name: this.$t('region.pan'), code: 'PAN' },
                        { name: this.$t('region.pur'), code: 'PUR' },
                        { name: this.$t('region.usa'), code: 'USA' }
                    ]
                },
                {
                    region: this.$t('region.oc'),
                    countries: [
                        { name: this.$t('region.aus'), code: 'AUS' },
                        { name: this.$t('region.nzl'), code: 'NZL' }
                    ]
                },
                {
                    region: this.$t('region.sa'),
                    countries: [
                        { name: this.$t('region.arg'), code: 'ARG' },
                        { name: this.$t('region.aru'), code: 'ARU' },
                        { name: this.$t('region.bol'), code: 'BOL' },
                        { name: this.$t('region.bra'), code: 'BRA' },
                        { name: this.$t('region.chi'), code: 'CHI' },
                        { name: this.$t('region.col'), code: 'COL' },
                        { name: this.$t('region.ecu'), code: 'ECU' },
                        { name: this.$t('region.par'), code: 'PAR' },
                        { name: this.$t('region.per'), code: 'PER' },
                        { name: this.$t('region.tto'), code: 'TTO' },
                        { name: this.$t('region.uru'), code: 'URU' },
                        { name: this.$t('region.ven'), code: 'VEN' }
                    ]
                }
            ]
        },
        emptyFilters() {
            return (
                this.minDate === '' &&
                this.maxDate === '' &&
                this.minDays === '' &&
                this.maxDays === '' &&
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
        minDays(newValue, oldValue) {
            if (newValue !== oldValue) {
                this.$emit('newMinDays', newValue)
            }
        },
        maxDays(newValue, oldValue) {
            if (newValue !== oldValue) {
                this.$emit('newMaxDays', newValue)
            }
        },
        regions(newValue, oldValue) {
            const valueCodes = newValue.map((res) => res.code)
            const valueCodesUniques = [...new Set(valueCodes)]

            if (valueCodesUniques !== oldValue) {
                this.$emit('newRegions', valueCodesUniques)
            }
        }
    },
    methods: {
        cleanFilters() {
            this.minDate = ''
            this.maxDate = ''
            this.minDays = ''
            this.maxDays = ''
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
    box-shadow: var(--shadow);
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
    margin-right: 4rem;
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
    position: relative;
    margin-left: 12px;
    background-color: var(--color-white);
    border: 1px solid var(--color-border);
    padding: 4px 12px;
    border-radius: var(--border-radius);
    cursor: pointer;
    width: 60px;
}
.navbar-filter:hover:enabled {
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

.active-filter {
    position: absolute;
    top: -4px;
    right: -4px;
    background: var(--color-info-dark);
    border: 1px solid var(--color-info-light);
    width: 12px;
    height: 12px;
    box-sizing: border-box;
    border-radius: 100%;
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
    margin-bottom: 20px;
}

.filter-item {
    display: inline-block;
    margin: 8px 24px 8px 0;
    color: var(--color-black-light);
    width: 288px;
}

.filter-item.min-date,
.filter-item.max-date,
.filter-item.min-days,
.filter-item.max-days {
    width: 150px;
}

.filter-item.regions {
    width: 100%;
    margin-bottom: 0;
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

input:hover,
select:hover {
    border-color: var(--color-border-hover);
}

@media only screen and (max-width: 524px) {
    .filter-item select,
    .filter-item.clean {
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
    position: relative;
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
