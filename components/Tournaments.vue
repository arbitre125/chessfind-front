<template>
    <div>
        <Navbar @displayMenu="displayMenu" />
        <div class="content">
            <div class="content-filters desktop">
                <div class="mobile langs">
                    <select v-model="$i18n.locale">
                        <option v-for="lang in langs" :key="lang" :value="lang">
                            {{ $t(`language.${lang}`) }}
                        </option>
                    </select>
                </div>

                <h2>{{ $t('filter.tournaments') }}</h2>
                <div class="filter">
                    <input
                        v-model="searchInput"
                        type="text"
                        :placeholder="$t('action.search') + '...'"
                        class="filter"
                        :disabled="loading"
                    />
                </div>
                <div class="filter">
                    <label>
                        {{ $t('filter.time_control') }}
                        <select v-model="timeControlType" :disabled="loading">
                            <option value="" selected>
                                {{ $t('action.select_option') }}
                            </option>
                            <option value="standard">
                                {{ $t('time_control.standard') }}
                            </option>
                            <option value="rapid">
                                {{ $t('time_control.rapid') }}
                            </option>
                            <option value="blitz">
                                {{ $t('time_control.blitz') }}
                            </option>
                        </select>
                    </label>
                </div>
                <div class="filter time">
                    <label>
                        {{ $t('filter.time_min') }}
                        <input
                            v-model="timeControlMin"
                            :placeholder="$t('minuts')"
                            type="number"
                            :min="0"
                            :disabled="loading"
                        />
                    </label>
                    <label>
                        {{ $t('filter.time_sec') }}
                        <input
                            v-model="timeControlSec"
                            :placeholder="$t('seconds')"
                            type="number"
                            :min="0"
                            :disabled="loading"
                        />
                    </label>
                </div>
                <div class="filter">
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
                <div class="filter">
                    <label>
                        {{ $t('filter.max_date') }}
                        <input
                            v-model="maxDate"
                            type="date"
                            :min="minDate"
                            :disabled="loading"
                        />
                    </label>
                </div>
                <div class="filter">
                    <label>
                        {{ $t('filter.min_days') }}
                        <input
                            v-model="minDays"
                            placeholder="1"
                            type="number"
                            :min="0"
                            :disabled="loading"
                        />
                    </label>
                </div>
                <div class="filter">
                    <label>
                        {{ $t('filter.max_days') }}
                        <input
                            v-model="maxDays"
                            placeholder="14"
                            type="number"
                            :min="minDays"
                            :disabled="loading"
                        />
                    </label>
                </div>
                <div class="filter">
                    <label>
                        {{ $t('valid_fide_elo') }}
                        <select
                            v-model="onlyValidByFIDEelo"
                            :disabled="loading"
                        >
                            <option value="">
                                {{ $t('action.select_option') }}
                            </option>
                            <option value="0">{{ $t('action.no') }}</option>
                            <option value="1">{{ $t('action.yes') }}</option>
                        </select>
                    </label>
                </div>
                <div class="filter">
                    <label>
                        {{ $t('filter.regions') }}
                        <multiselect
                            v-model="displayRegions"
                            :placeholder="
                                $t('action.search') +
                                ' ' +
                                $t('filter.regions').toLowerCase()
                            "
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
                            <span slot="noResult"
                                >{{ $t('error.no_result_found') }}.</span
                            >
                        </multiselect>
                    </label>
                </div>
                <div class="filter">
                    <button
                        class="clean"
                        :disabled="emptyFilters || loading"
                        @click="cleanFilters"
                    >
                        {{ $t('action.clean_filters') }}
                    </button>
                </div>
            </div>
            <div v-if="displayMobileMenu" class="content-filters mobile">
                <div class="mobile langs">
                    <select v-model="$i18n.locale">
                        <option v-for="lang in langs" :key="lang" :value="lang">
                            {{ $t(`language.${lang}`) }}
                        </option>
                    </select>
                </div>

                <h2>{{ $t('filter.tournaments') }}</h2>
                <div class="filter">
                    <input
                        v-model="searchInput"
                        type="text"
                        :placeholder="$t('action.search') + '...'"
                        class="filter"
                        :disabled="loading"
                    />
                </div>
                <div class="filter">
                    <label>
                        {{ $t('filter.time_control') }}
                        <select v-model="timeControlType" :disabled="loading">
                            <option value="">
                                {{ $t('action.select_option') }}
                            </option>
                            <option value="standard">
                                {{ $t('time_control.standard') }}
                            </option>
                            <option value="rapid">
                                {{ $t('time_control.rapid') }}
                            </option>
                            <option value="blitz">
                                {{ $t('time_control.blitz') }}
                            </option>
                        </select>
                    </label>
                </div>
                <div class="filter time">
                    <label>
                        {{ $t('filter.time_min') }}
                        <input
                            v-model="timeControlMin"
                            :placeholder="$t('minuts')"
                            type="number"
                            :min="0"
                            :disabled="loading"
                        />
                    </label>
                    <label>
                        {{ $t('filter.time_sec') }}
                        <input
                            v-model="timeControlSec"
                            :placeholder="$t('seconds')"
                            type="number"
                            :min="0"
                            :disabled="loading"
                        />
                    </label>
                </div>
                <div class="filter">
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
                <div class="filter">
                    <label>
                        {{ $t('filter.max_date') }}
                        <input
                            v-model="maxDate"
                            type="date"
                            :min="minDate"
                            :disabled="loading"
                        />
                    </label>
                </div>
                <div class="filter">
                    <label>
                        {{ $t('filter.min_days') }}
                        <input
                            v-model="minDays"
                            placeholder="1"
                            type="number"
                            :min="0"
                            :disabled="loading"
                        />
                    </label>
                </div>
                <div class="filter">
                    <label>
                        {{ $t('filter.max_days') }}
                        <input
                            v-model="maxDays"
                            placeholder="14"
                            type="number"
                            :min="minDays"
                            :disabled="loading"
                        />
                    </label>
                </div>
                <div class="filter">
                    <label>
                        {{ $t('valid_fide_elo') }}
                        <select
                            v-model="onlyValidByFIDEelo"
                            :disabled="loading"
                        >
                            <option value="">
                                {{ $t('action.select_option') }}
                            </option>
                            <option value="0">{{ $t('action.no') }}</option>
                            <option value="1">{{ $t('action.yes') }}</option>
                        </select>
                    </label>
                </div>
                <div class="filter">
                    <label>
                        {{ $t('filter.regions') }}
                        <multiselect
                            v-model="displayRegions"
                            :placeholder="
                                $t('action.search') +
                                ' ' +
                                $t('filter.regions').toLowerCase()
                            "
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
                            <span slot="noResult"
                                >{{ $t('error.no_result_found') }}.</span
                            >
                        </multiselect>
                    </label>
                </div>
                <div class="filter">
                    <button
                        class="clean"
                        :disabled="emptyFilters || loading"
                        @click="cleanFilters"
                    >
                        {{ $t('action.clean_filters') }}
                    </button>
                </div>
            </div>
            <div class="content-tournaments">
                <div v-if="loading">
                    <TournamentSkeletonCard class="header-info" />
                    <TournamentSkeletonCard
                        v-for="i in displayPerPage"
                        :key="i"
                    />
                </div>
                <div v-else-if="error" class="message error">
                    <img
                        src="@/assets/icons/error.svg"
                        class="placeholder-icon"
                    />
                    <h2>{{ $t('error.error_found') }}</h2>
                </div>
                <div v-else-if="!tournaments.length" class="message info">
                    <img
                        src="@/assets/icons/no_data.svg"
                        class="placeholder-icon"
                    />
                    <h2>{{ $t('error.no_result_found') }}</h2>
                </div>
                <div v-else>
                    <div class="header-info">
                        <div class="header-results">
                            <b>{{ totalTournaments }}</b>
                            {{ $t('results').toLowerCase() }}
                        </div>
                        <div class="header-sorting">
                            <div class="sorting">
                                {{ $t('sort_by') }}
                                <select v-model="sorting">
                                    <option
                                        v-for="item in sortOptions"
                                        :key="item.display"
                                        :value="{
                                            value: item.value,
                                            dir_desc: item.desc
                                        }"
                                    >
                                        {{ item.display }}
                                    </option>
                                </select>
                            </div>
                        </div>
                    </div>
                    <TournamentCard
                        v-for="tournament of tournaments"
                        :key="tournament.link"
                        :tournament="tournament"
                    />
                    <div v-if="maxPages > 1" class="pagination">
                        <button
                            class="pagination-btn"
                            :disabled="currentPage == 1"
                            @click="prevPage"
                        >
                            <img src="@/assets/icons/arrow_left.svg" />
                        </button>
                        <input
                            v-model="currentPage"
                            type="number"
                            :min="1"
                            :max="maxPages"
                            @change="updatePage"
                        />
                        / {{ maxPages }}
                        <button
                            class="pagination-btn"
                            :disabled="currentPage == maxPages"
                            @click="nextPage()"
                        >
                            <img src="@/assets/icons/arrow_right.svg" />
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import Multiselect from 'vue-multiselect'

export default {
    components: {
        Multiselect
    },
    data() {
        return {
            loading: false,
            error: false,
            tournaments: [],
            totalTournaments: 0,
            displayPerPage: 10,
            currentPage: 1,
            sorting: {
                value: 'start',
                dir_desc: false
            },
            searchInput: '',
            minDate: '',
            maxDate: '',
            minDays: '',
            maxDays: '',
            regions: [],
            displayRegions: [],
            onlyValidByFIDEelo: '',
            timeControlType: '',
            timeControlMin: '',
            timeControlSec: '',
            awaitingInput: false,
            displayMobileMenu: false
        }
    },
    async fetch() {
        this.loading = true

        this.setParamsFromRouter()

        await this.$axios
            .post(this.$config.apiURL + '/tournaments', this.getParams)
            .then((response) => {
                this.tournaments = response.data.tournaments
                this.totalTournaments = response.data.total
                this.loading = false
            })
            .catch(() => {
                this.loading = false
                this.error = true
            })
    },
    computed: {
        langs() {
            return ['en', 'es']
        },
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
                        { name: this.$t('region.cat'), code: 'CAT' },
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
        maxPages() {
            return Math.ceil(this.totalTournaments / this.displayPerPage)
        },
        sortOptions() {
            return [
                { value: 'start', desc: false, display: 'Soonest start day' },
                { value: 'start', desc: true, display: 'Latest start day' },
                { value: 'end', desc: false, display: 'Soonest end day' },
                { value: 'end', desc: true, display: 'Latest end day' },
                { value: 'rounds', desc: true, display: 'More rounds' },
                { value: 'rounds', desc: false, display: 'Less rounds' },
                {
                    value: 'days_duration',
                    desc: true,
                    display: 'More duration'
                },
                {
                    value: 'days_duration',
                    desc: false,
                    display: 'Less duration'
                },
                {
                    value: 'average_elo',
                    desc: true,
                    display: 'More average ELO'
                },
                {
                    value: 'average_elo',
                    desc: false,
                    display: 'Less average ELO'
                }
            ]
        },
        getParams() {
            const params = {}
            params.mode = 'cors'
            params.headers = {
                'Access-Control-Allow-Origin': '*'
            }
            params.display_per_page = this.displayPerPage
            params.current_page = this.currentPage
            params.sorting_value =
                this.$route.query.sort_value || this.sorting.value
            params.sorting_dir_desc =
                this.$route.query.sort_desc || this.sorting.dir_desc
            params.search_text = this.searchInput
            params.regions = this.regions
            params.only_valid_fide = this.onlyValidByFIDEelo
            params.time_control_type = this.timeControlType
            params.time_control_min = this.timeControlMin
            params.time_control_sec = this.timeControlSec

            if (this.minDate !== '') {
                params.min_date = this.formatDate(this.minDate)
            }
            if (this.maxDate !== '') {
                params.max_date = this.formatDate(this.maxDate)
            }
            if (this.minDays !== '') {
                params.min_days = this.minDays
            }
            if (this.maxDays !== '') {
                params.max_days = this.maxDays
            }

            return params
        },
        emptyFilters() {
            return (
                this.searchInput === '' &&
                this.minDate === '' &&
                this.maxDate === '' &&
                this.minDays === '' &&
                this.maxDays === '' &&
                this.onlyValidByFIDEelo === '' &&
                this.timeControlType === '' &&
                this.timeControlMin === '' &&
                this.timeControlSec === '' &&
                this.displayRegions.length === 0
            )
        }
    },
    watch: {
        displayPerPage() {
            this.updateValue()
        },
        sorting() {
            this.$router.push({
                query: {
                    sort_value: this.sorting.value,
                    sort_desc: this.sorting.dir_desc
                }
            })
            this.updateValue()
        },
        searchInput() {
            if (!this.awaitingInput) {
                setTimeout(() => {
                    this.$fetch()
                    this.awaitingInput = false
                }, 1000) // 1 sec delay
            }
            this.awaitingInput = true
        },
        timeControlType() {
            this.updateValue()
        },
        minDate() {
            this.updateValue()
        },
        maxDate() {
            this.updateValue()
        },
        minDays() {
            if (!this.awaitingInput) {
                setTimeout(() => {
                    this.$fetch()
                    this.awaitingInput = false
                }, 1000) // 1 sec delay
            }
            this.awaitingInput = true
        },
        maxDays() {
            if (!this.awaitingInput) {
                setTimeout(() => {
                    this.$fetch()
                    this.awaitingInput = false
                }, 1000) // 1 sec delay
            }
            this.awaitingInput = true
        },
        onlyValidByFIDEelo() {
            this.updateValue()
        },
        timeControlMin() {
            if (!this.awaitingInput) {
                setTimeout(() => {
                    this.$fetch()
                    this.awaitingInput = false
                }, 1000) // 1 sec delay
            }
            this.awaitingInput = true
        },
        timeControlSec() {
            if (!this.awaitingInput) {
                setTimeout(() => {
                    this.$fetch()
                    this.awaitingInput = false
                }, 1000) // 1 sec delay
            }
            this.awaitingInput = true
        },
        displayRegions(newValue) {
            const valueCodes = newValue.map((res) => res.code)
            this.regions = [...new Set(valueCodes)]
            this.updateValue()
        }
    },
    methods: {
        displayMenu(value) {
            this.displayMobileMenu = value
        },
        prevPage() {
            if (this.currentPage > 0) {
                this.currentPage -= 1
                this.$fetch()
            }
        },
        nextPage() {
            if (this.currentPage <= this.maxPages) {
                this.currentPage += 1
                this.$fetch()
            }
        },
        updatePage(event) {
            const value = event.target.value
            if (parseInt(value) < 1) {
                this.currentPage = 1
            } else if (parseInt(value) > this.maxPages) {
                this.currentPage = this.maxPages
            }
            if (!this.awaitingInput) {
                setTimeout(() => {
                    this.$fetch()
                    this.awaitingInput = false
                }, 1000) // 1 sec delay
            }
            this.awaitingInput = true
        },
        updateValue() {
            this.currentPage = 1
            this.$fetch()
        },
        setParamsFromRouter() {
            this.sorting.value = this.$route.query.sort_value
                ? this.$route.query.sort_value
                : 'start'
            this.sorting.dir_desc = this.$route.query.sort_desc
                ? this.$route.query.sort_desc
                : false

            if (this.$route.params.search_input !== '') {
                this.searchInput = this.$route.params.search_input
                    ? this.$route.params.search_input
                    : ''
                this.$route.params.search_input = ''
            }
            if (this.$route.params.min_date !== '') {
                this.minDate = this.$route.params.min_date
                    ? this.$route.params.min_date
                    : ''
                this.$route.params.min_date = ''
            }
            if (this.$route.params.max_date !== '') {
                this.maxDate = this.$route.params.max_date
                    ? this.$route.params.max_date
                    : ''
                this.$route.params.max_date = ''
            }
            if (this.$route.params.time_control_type !== '') {
                this.timeControlType = this.$route.params.time_control_type
                    ? this.$route.params.time_control_type
                    : ''
                this.$route.params.time_control_type = ''
            }
            if (
                this.$route.params.time_control_min !== undefined &&
                this.$route.params.time_control_min !== ''
            ) {
                this.timeControlMin =
                    this.$route.params.time_control_min.toString()
                this.$route.params.time_control_min = ''
            }
            if (
                this.$route.params.time_control_sec !== undefined &&
                this.$route.params.time_control_sec !== ''
            ) {
                this.timeControlSec =
                    this.$route.params.time_control_sec.toString()
                this.$route.params.time_control_sec = ''
            }
            if (
                this.$route.params.regions !== undefined &&
                this.$route.params.regions !== ''
            ) {
                this.regions = this.$route.params.regions
                const region = this.regions[0]
                this.displayRegions = [
                    {
                        name: this.$t(`region.${region.toLowerCase()}`),
                        code: region.toUpperCase()
                    }
                ]
                this.$route.params.regions = ''
            }
        },
        cleanFilters() {
            this.searchInput = ''
            this.minDate = ''
            this.maxDate = ''
            this.minDays = ''
            this.maxDays = ''
            this.onlyValidByFIDEelo = ''
            this.timeControlType = ''
            this.timeControlMin = ''
            this.timeControlSec = ''
            this.displayRegions = []
            this.regions = []
            this.updateValue()
        },
        formatDate(date) {
            const d = new Date(date)
            let month = '' + (d.getMonth() + 1)
            let day = '' + d.getDate()
            const year = d.getFullYear()

            if (month.length < 2) month = '0' + month
            if (day.length < 2) day = '0' + day

            return [day, month, year].join('/')
        }
    }
}
</script>

<style scoped>
h2 {
    margin-bottom: -16px;
}
.message {
    text-align: center;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 400px;
    border-radius: var(--border-radius);
}

.placeholder-icon {
    max-height: 200px;
    max-width: 400px;
    margin-bottom: 12px;
}

.header-info {
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: 32px !important;
    max-height: 32px !important;
}

.header-sorting {
    display: inline-flex;
}

.sorting {
    margin-left: 10px;
}

.pagination {
    display: flex;
    justify-content: center;
    align-items: center;
}

.pagination input {
    margin-right: 8px;
}

.pagination-btn {
    margin: 4px 16px;
    background-color: var(--color-white);
    border: 1px solid var(--color-border);
    border-radius: var(--border-radius);
    font-weight: bold;
}
.pagination-btn[disabled] {
    opacity: 50%;
}

.pagination-btn img {
    width: 22px;
    padding-top: 2px;
}
.pagination-btn:hover:enabled,
.pagination input:hover {
    border-color: var(--color-border-hover);
    cursor: pointer;
}

.content {
    margin: 20px 4%;
    display: flex;
    gap: 32px;
}

.content-filters {
    width: 400px;
    border-radius: 8px;
    padding: 4px 0;
}

@media only screen and (max-width: 769px) {
    .content-filters {
        overflow: hidden;
        position: absolute;
        top: 60px;
        background-color: var(--color-background);
        height: 100vh;
        width: 96%;
        padding-right: 4%;
        border-radius: 0;
        padding-bottom: 20px;
    }
}

.content-tournaments {
    width: 100%;
}

.filter {
    margin-top: 16px;
    margin-bottom: 4px;
    display: flex;
    flex-direction: column;
}

.filter.time {
    flex-direction: row;
    flex-wrap: nowrap;
    gap: 16px;
    margin-right: 16px;
}

.filter.time label {
    flex: 1 0 50%;
}

.filter input,
.filter select,
.filter button {
    width: 100%;
}

.clean {
    margin-top: 20px;
}

.multiselect {
    display: block;
    width: 100%;
    cursor: pointer;
}

label input,
label select,
label .multiselect {
    margin-top: 4px;
}

.langs {
    margin-bottom: 20px;
}
.langs select {
    width: 100%;
}
</style>
