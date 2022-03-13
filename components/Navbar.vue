<template>
    <div class="navbar">
        <div class="content-container">
            <nuxt-link to="/">
                <Logo class="navbar-logo" />
            </nuxt-link>
            <div class="navbar-right mobile">
                <div
                    v-if="menu"
                    class="icon-menu"
                    @click="displayMenu = !displayMenu"
                >
                    <IconClose v-if="displayMenu" />
                    <IconMenu v-else />
                </div>
                <select v-else v-model="$i18n.locale">
                    <option v-for="lang in langs" :key="lang" :value="lang">
                        {{ $t(`language.${lang}`) }}
                    </option>
                </select>
            </div>
            <div class="navbar-right desktop">
                <select v-model="$i18n.locale">
                    <option v-for="lang in langs" :key="lang" :value="lang">
                        {{ $t(`language.${lang}`) }}
                    </option>
                </select>
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
    props: {
        loading: {
            type: Boolean,
            default: false
        },
        menu: {
            type: Boolean,
            default: true
        }
    },
    data() {
        return {
            displayMenu: false
        }
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
        }
    },
    watch: {
        displayMenu() {
            this.$emit('displayMenu', this.displayMenu)
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
    padding: 2px 0;
    display: flex;
    width: 92%;
    margin: 0 4%;
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
    flex-direction: row-reverse;
}

.navbar-right.mobile {
    flex-direction: row;
    justify-content: flex-end;
    align-items: center;
}

.icon-menu {
    position: relative;
    width: 24px;
    height: 24px;
    margin-right: 4px;
    cursor: pointer;
}
</style>
