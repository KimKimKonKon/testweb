<template>
  <client-only>
    <div id="supportBu" class="support-bu">
      <ul class="breadcrumb">
        <li class="index">
          <a class="breadcrumb-link" :href="`/${ locale }/support`">{{ $t('overviewPage.title.support') }}</a>
        </li>
        <li class="current">
          <span>｜</span>
          <a class="breadcrumb-link">
            <span v-if="currentBuSelect === 'xpg'">{{ $t('breadcrumb.main.content1') }}</span>
            <span v-else-if="currentBuSelect === 'consumer'">{{ $t('breadcrumb.main.content2') }}</span>
            <span v-else-if="currentBuSelect === 'industrial'">{{ $t('breadcrumb.main.content5') }}</span>
            <span v-else-if="currentBuSelect === 'lighting'">{{ $t('breadcrumb.main.content4') }}</span>
            <span v-else>{{ $t('breadcrumb.main.content3') }}</span>
          </a>
        </li>
      </ul>
      <!-- PC view -->
      <section class="tab-container" v-if="tabs.length > 0 && !(windowWidth <= xs)">
        <tab 
          :tabs="tabs" 
          :currentTab='currentTabName' 
          :handleClick="handleClick" 
          :styleObject="{
            width: '85.4vw',
            padding: '1.592vw 0 1.229vw'
          }" 
          lineClass="default-tabs__active-line-support"
        >        
          <template v-for="(tabsItem, idx) in tabs" :slot="tabsItem.value">
            <!--{{ tabsItem.value }}-->

            <!-- Download -->
            <div class="support-container" v-if="tabsItem.value === 'downloads'">
              <download :downloadTabData="downloadTabData" :bu="currentBuSelect" :softwareData="currentTabData" :setDownloadTab="tabsItem.value === 'downloads' ? downloadTabName : 'software' "></download>
            </div>
            <!-- FAQ -->
            <div class="support-container" v-if="tabsItem.value === 'faqs' ">
              <faq :tabs="faqTab" :bu="currentBuSelect" :faqData="currentTabData.length && currentTabData.length > 0 ? currentTabData : []" />
            </div>
            <!-- Contact -->
            <div class="support-container" v-if="tabsItem.value === 'services' ">
              <contact :contactData="currentTabData" />
            </div>
            <!-- Warranty -->
            <div class="support-container" v-if="tabsItem.value === 'warranty' ">
              <warranty :bu="currentBuSelect" :warrantyData="currentTabData" :setWarrantyTab="tabsItem.value === 'warranty' ? warrantyTabName : 'warrantySearch' " />
            </div>
            <!-- Technical Supports -->
            <div class="support-container" v-if="tabsItem.value === 'technicals'">
              <technical-supports :bu="currentBuSelect" :dataSavingData="currentTabData.length && currentTabData.length > 0 ? currentTabData : []" :setTechnicalTab="tabsItem.value === 'technicals' ? technicalSupportsTabName : 'dataSaving'" />
            </div>

            <!-- Repair Search -->
            <div class="support-container" v-if="tabsItem.value === 'repair_search'">
              <repair-search />
            </div>

            <!-- Anti Forgery -->
            <div class="support-container" v-if="tabsItem.value === 'anti_forgeries'">
              <anti-forgery :list="currentTabData.length && currentTabData.length > 0 ? currentTabData : []" />
            </div>

            <!-- PCN & EOL Information 
            <div class="support-container" v-if="tabsItem.value === 'technicals' && pcn">
              <pcn-eol :list="pcnList" />
            </div>-->
          </template>
        </tab>
      </section>

      <!-- Mobile view -->
        <section class="dropdown-container" v-if="tabs.length > 0 && (windowWidth <= xs)">
          <dropdown-tabs :tabs="tabs" :currentTab='currentTabName' :isOpen="isMobileTabsMenuOpen" @handleClick="handleClick"></dropdown-tabs>

          <!-- Download -->
          <div class="support-container" v-if="currentTabName === 'downloads'">
            <download :downloadTabData="downloadTabData" :bu="currentBuSelect" :softwareData="currentTabData" :setDownloadTab="currentTabName === 'downloads' ? downloadTabName : 'software' "></download>
          </div>
          <!-- Contact -->
          <div class="support-container" v-if="currentTabName === 'services' ">
            <contact :contactData="currentTabData" />
          </div>

          <!-- FAQ -->
          <div class="support-container" v-if="currentTabName === 'faqs' ">
            <faq :tabs="faqTab" :bu="currentBuSelect" :faqData="currentTabData.length && currentTabData.length > 0 ? currentTabData : []" />
          </div>

          <!-- Warranty -->
          <div class="support-container" v-if="currentTabName === 'warranty' ">
            <warranty :bu="currentBuSelect" :warrantyData="currentTabData" :setWarrantyTab="currentTabName === 'warranty' ? warrantyTabName : 'warrantySearch' " />
          </div>

          <!-- Technical Supports -->
          <div class="support-container" v-if="currentTabName === 'technicals'">
            <technical-supports :bu="currentBuSelect" :dataSavingData="currentTabData.length && currentTabData.length > 0 ? currentTabData : []" :setTechnicalTab="currentTabName === 'technicals' ? technicalSupportsTabName : 'dataSaving'" />
          </div>

          <!-- Repair Search -->
          <div class="support-container" v-if="currentTabName === 'repair_search'">
            <repair-search />
          </div>

          <!-- Anti Forgery -->
          <div class="support-container" v-if="currentTabName === 'anti_forgeries'">
            <anti-forgery :list="currentTabData.length && currentTabData.length > 0 ? currentTabData : []" />
          </div>

          <!-- PCN & EOL Information 
          <div class="support-container" v-if="currentTabName === 'technicals' && pcn">
            <pcn-eol :list="pcnList" />
          </div>-->
          
        </section>
      
      <modal-component v-if="currentTabName === 'services' " :width='54.688' :height='28' closeBtn class="pc-show">
        <div v-html="servicesAlertMessage.replaceAll('style', 'data').replaceAll('<a', `<a style='color: #ff345c;'`)">
        </div>
      </modal-component>

      <modal-component v-if="currentTabName === 'services' " :height='20' closeBtn class="mobile-show">
        <div v-html="servicesAlertMessage.replaceAll('style', 'data').replaceAll('<a', `<a style='color: #ff345c;'`)">
        </div>
      </modal-component>
    </div>
  </client-only>
</template>

<script>

import ModalComponent from '~/components/shared/ModalComponent';
import DropdownTabs from '@/components/shared/DropdownTabs.vue';
import Tab from '@/components/shared/Tabs.vue';
import Download from '@/components/support/Download.vue';
import Contact from '@/components/support/Contact.vue';
import Warranty from '@/components/support/Warranty.vue';
import Faq from '@/components/support/Faq.vue';
import TechnicalSupports from '@/components/support/TechnicalSupports.vue';
import RepairSearch from '@/components/support/RepairSearch.vue';
import AntiForgery from '@/components/support/AntiForgery.vue';
import PcnEol from '@/components/support/PcnEol.vue';
import { mapState, mapMutations } from 'vuex';

import { APP_NAME } from '@/plugins/const.js';

export default {
  name: 'SupportBu',
  components: {
    ModalComponent,
    Tab,
    DropdownTabs,
    Download,
    Contact,
    Warranty,
    Faq,
    TechnicalSupports,
    RepairSearch,
    AntiForgery,
    PcnEol
  },
  async asyncData({ $axios, params, query, redirect, error }) {
    if (!query.tab ) {
      redirect(`/${ params.lang }/support/${ params.bu }?tab=downloads`);
    } else {
      const tab = query.tab ? query.tab : 'downloads';
      $axios.setHeader('X-localization', params.lang && params.lang !== 'en' ? params.lang : 'us');
      const path = `${ process.env.API_URL }/api/page/bu/${ params.bu }/support_sections`;
      const tabQueryPath = `${ process.env.API_URL }/api/page/bu/${ params.bu }/${ tab }`;
      const faqPath = `${ process.env.API_URL }/api/page/bu/${ params.bu }/categories`;
      const downloadTabPath = `${ process.env.API_URL }/api/page/bu/${ params.bu }/tag_downloads`;
      let supportTabsResonse;
      let supportTabData;
      let faqTabResponse;
      let downloadsTabResponse;
      // if (query.tab !== 'repair_search') {
        try {
          downloadsTabResponse = await $axios.get(downloadTabPath);
          supportTabsResonse = await $axios.get(path);
          let currentTabData;
          if (query.tab !== 'repair_search') {
            supportTabData = await $axios.get(tabQueryPath);
            currentTabData = supportTabData.data.data;
          } else {
            currentTabData = [];
          }
          const downloadTabData = downloadsTabResponse.data.data;
          const buTabs = supportTabsResonse.data.data;
          
          let faqTab = [];
          if (tab === 'faqs') {
            faqTabResponse = await $axios.get(faqPath);
            for (let idx = 0; idx < faqTabResponse.data.data.length; idx++) {
              const tab = {
                title: faqTabResponse.data.data[idx].text,
                val: faqTabResponse.data.data[idx].id
              };
              faqTab.push(tab);
            }
          }
          return {
            buTabs,
            currentTabData,
            faqTab,
            downloadTabData
          }
        } catch(err) {
          console.log(err);
          error({ statusCode: 404, message: 'page not found' });
        }
      //}
    }
  },
  data() {
    return {
      apiUrl: process.env.API_URL,
      buTabs: [],
      currentTabData: [],
      faqTab: [],
      lastScrollY: 0,
      isMobileTabsMenuOpen: false,
      currentTabName: 'downloads',
      downloadTabName: 'software',
      warrantyTabName: 'warrantySearch',
      technicalSupportsTabName: 'dataSaving',
      pcn: false,
      servicesAlertMessage: '',
      b2bTabs: [
        {
          title: '產品保固',
          value: 'warranty',
        },
        {
          title: '售後服務',
          value: 'afterSales',
        },
        {
          title: 'PCN & EOL 說明',
          value: 'technicals',
        },
        {
          title: '常見問題',
          value: 'faqs',
        },
        {
          title: '下載中心',
          value: 'downloads',
        }
      ],
      b2cTabs: [
        {
          title: '下載中心',
          value: 'downloads',
        },
        {
          title: '線上報修',
          value: 'repair_online',
        },
        {
          title: '聯絡客服',
          value: 'services',
        }, 
        {
          title: '產品保固',
          value: 'warranty',
        }, 
        {
          title: '常見問題',
          value: 'faqs',
        },
        {
          title: '產品註冊',
          value: 'product_register',
        },
        {
          title: '技術支援',
          value: 'technicals',
        },
        {
          title: '維修進度查詢',
          value: 'repair_search',
        },
        {
          title: '防偽措施',
          value: 'anti_forgeries',
        }
      ],
      windowWidth: 0
    };
  },
  head() {
    return {
      titleTemplate: `${ this.supportTitle } | ADATA`,
      meta: [
        { hid: 'description', name: 'description', content: this.supportTitle },
        { property: 'og:image', content: 'https://corp.adata.com/_nuxt/img/about_header.8044040.png'},
        { property: 'og:title', content: 'ADATA' },
        { property: 'og:description', content: this.supportTitle },
      ],
      __dangerouslyDisableSanitizers: ['script'],
      script: [
        {
          hid: 'gtm-script1',
          src: `https://www.googletagmanager.com/gtag/js?id=G-QX1LG3N75L`,
          defer: true
        },
        {
          hid: 'gtm-script2',
          innerHTML: `
            window.dataLayer = window.dataLayer || [];
            function gtag(){dataLayer.push(arguments);}
            gtag('js', new Date());

            gtag('config', 'G-QX1LG3N75L');
          `,
          type: 'text/javascript',
          charset: 'utf-8'
        }
      ]
    }
  },
  created() {
    if (this.$route.params.bu === 'industrial' || this.$route.params.bu === 'powertrain') {
      this.currentTabName = this.$route.query.tab ? this.$route.query.tab : 'warranty';
    } else {
      this.currentTabName = this.$route.query.tab ? this.$route.query.tab : 'downloads';
    }
    
    if (this.$route.params.bu === 'xpg' || this.$route.params.bu === 'consumer') {
      this.warrantyTabName = this.$route.query.warranty ? this.$route.query.warranty : 'warrantySearch';
    } else {
      this.warrantyTabName = this.$route.query.warranty ? this.$route.query.warranty : 'warrantyService';
    }
    this.downloadTabName = this.$route.query.download ? this.$route.query.download : 'software';
    
    this.technicalSupportsTabName = this.$route.query.technical ? this.$route.query.technical : 'dataSaving';
    this.setCurrentBuSelect(this.$route.params.bu);
    this.onResize();
  },
  mounted() {
    this.triggerToStartScrollAnimation();
    if (this.$route.query.tab === 'services') {
      this.getSupportAlert(this.$route.params.bu);
    }
    this.$nextTick(() => {
      window.addEventListener('resize', this.onResize);
    });
  },
  beforeDestroy() {
    window.removeEventListener('scroll', this.scrollAnimationController);
    window.removeEventListener('resize', this.onResize);
  },
  watch: {
    $route(newValue, oldValue) {
      const oldTabSelect = oldValue.query.tab;
      const tabSelect = newValue.query.tab;
      if (oldTabSelect !== tabSelect) {
        this.getTabData(tabSelect);
      }
      
      if (tabSelect === 'services') {
        this.getSupportAlert(this.$route.params.bu);
      }
    }
  },
  computed: {
    ...mapState('support', ['supportTitle']),
    ...mapState(['currentBuSelect', 'xs', 'locale']),
    tabs() {
      const bu = this.$route.params.bu;
      if (bu === 'industrial' || bu === 'powertrain') {
        const tabsData = [...this.buTabs];
        /*const arrayTabs = [...this.buTabs];
        arrayTabs.map(e => {
          e.value = e.api_path;
        });
        arrayTabs.push({
          title: '售後服務',
          value: 'afterSales',
        });
        
        return arrayTabs;*/
        let arrayTabs = [];
        tabsData.filter(e => e.api_path === 'warranty_policies').length > 0 ? arrayTabs.push({
          title: this.$t('supportPage.tab.warranty'),
          value: 'warranty',
        }) : null;
        tabsData.filter(e => e.api_path === 'services').length > 0 ? arrayTabs.push({
          title:  this.$t('supportPage.tab.onlineRmaSystem'),
          value: 'afterSales',
        }) : null;
        tabsData.filter(e => e.api_path === 'technicals').length > 0 ? arrayTabs.push({
          title: this.$t('supportPage.tab.policy'),
          value: 'technicals',
        }) : null;
        tabsData.filter(e => e.api_path === 'faqs').length > 0 ? arrayTabs.push({
          title: this.$t('supportPage.tab.faq'),
          value: 'faqs',
        }) : null;
        tabsData.filter(e => e.api_path === 'downloads').length > 0 ? arrayTabs.push({
          title: this.$t('supportPage.tab.download'),
          value: 'downloads',
        }) : null;
        return arrayTabs;
      } else {
        const tabsData = [...this.buTabs];
        let arrayTabs = [];
        tabsData.filter(e => e.api_path === 'downloads').length > 0 ? arrayTabs.push({
          title: this.$t('supportPage.tab.download'),
          value: 'downloads',
        }) : null;
        
        if (this.locale === 'us' || this.locale === 'tw') {
          tabsData.filter(e => e.title === 'Submit RMA Request').length > 0  ? arrayTabs.push({
            title: this.$t('supportPage.tab.submitRmaRequest'),
            value: 'repair_online',
          }) : null;
        }
        
        tabsData.filter(e => e.api_path === 'services').length > 0 ? arrayTabs.push({
          title: this.$t('supportPage.tab.serviceCenter'),
          value: 'services',
        }) : null;
        tabsData.filter(e => e.api_path === 'warranty_policies').length > 0 ? arrayTabs.push({
          title: this.$t('supportPage.tab.warrantyInformation'),
          value: 'warranty',
        }) : null;
        tabsData.filter(e => e.api_path === 'faqs').length > 0 ? arrayTabs.push({
          title: this.$t('supportPage.tab.faq'),
          value: 'faqs',
        }) : null;
        //
        tabsData.filter(e => e.title === 'Product Registration').length > 0 ? arrayTabs.push({
          title: this.$t('supportPage.tab.productRegistration'),
          value: 'product_register',
        }) : null;
        
        tabsData.filter(e => e.api_path === 'technicals').length > 0 ? arrayTabs.push({
          title: this.$t('supportPage.tab.technicalSupport'),
          value: 'technicals',
        }) : null;
        tabsData.filter(e => e.title === 'PRMA Process').length > 0 ? arrayTabs.push({
          title: this.$t('supportPage.tab.serviceStatusInquiry'),
          value: 'repair_search',
        }) : null;
        tabsData.filter(e => e.api_path === 'anti_forgeries').length > 0 ? arrayTabs.push({
          title: this.$t('supportPage.tab.antiCounterfeitInitiatives'),
          value: 'anti_forgeries',
        }) : null;
        return arrayTabs;
      }
    }
  },
  methods: {
    ...mapMutations(['setCurrentBuSelect']),
    /**
     * [ handleClick function ]
     * @param  { String } newTab [ new tab name ]
     */
    handleClick(newTab) {
      this.isMobileTabsMenuOpen = this.isMobileTabsMenuOpen ? false : true;
      if (newTab === 'repair_online') {
        this.$router.push({
          name: 'lang-member'
        });
      } else if (newTab === 'product_register') {
        this.$router.push({
          name: 'lang-member'
        });
      } else if (newTab === 'afterSales') {
        window.location = 'http://onlinerma.adata.com/Login.aspx?ReturnUrl=%2f';
      } else if(newTab ==='services'){
          if( this.locale !== 'tw'){
            window.open(`http://corp.adata.com/${this.locale}/support/online`);
          }
        this.$router.push({
          name: 'lang-support-bu',
          query: {
            tab: newTab
          }
        });
        this.currentTabName = newTab;
      }else {
        this.$router.push({
          name: 'lang-support-bu',
          query: {
            tab: newTab
          }
        });
        this.currentTabName = newTab;
      }
    },
    /**
     * [Trigger to start scroll animation function]
     */
    triggerToStartScrollAnimation() {
      this.$bus.$emit('setNavbarStatus', {
        status: 'up'
      });
      window.addEventListener('scroll', this.scrollAnimationController);
    },
    /**
     * [scrollAnimationController function]
     * @description: control the animation in scroll event.
     */
    scrollAnimationController() {
      let value = window.pageYOffset;
      if (value >= document.body.clientHeight - window.innerHeight) {
        this.triggerBottom = true;
      }

      if (value < 0) {
        this.triggerTop = true;
      }

      if (value <= this.lastScrollY && !this.triggerBottom) {
        this.$bus.$emit('setNavbarStatus', {
          status: 'up'
        });
      } else if (value < this.lastScrollY - 10 && this.triggerBottom) {
        this.triggerBottom = false;
      } else if (this.triggerBottom && !this.triggerTop) {
        this.$bus.$emit('setNavbarStatus', {
          status: 'down'
        });
      } else if (value > this.lastScrollY + 5 && this.triggerTop) {
        this.triggerTop = false;
      } else if (value > this.lastScrollY && this.triggerTop) {
        this.$bus.$emit('setNavbarStatus', {
          status: 'up'
        });
      } else {
        this.$bus.$emit('setNavbarStatus', {
          status: 'down'
        });
      }
      this.lastScrollY = value;
    },
    /**
     * [ Tab API  ]
     * @param  { String } tabKey [ tab select key value ]
     */
    async getTabData(tabKey) {
      if (tabKey !== 'repair_search') {
        const path = `${ this.apiUrl }/api/page/bu/${ this.$route.params.bu }/${ tabKey }`;
        let response;
        try {
          response = await this.$axios.get(path);
          this.currentTabData = response.data.data;
        } catch (err) {
          this.$router.push({
            name: 'error'
          });
        }
      }

      if (tabKey === 'faqs') {
        this.getTabList(this.currentBuSelect);
      }
    },
    async getTabList(key) {
      const path = `${ this.apiUrl }/api/page/bu/${ key }/categories`;
      let res;
      try {
        res = await this.$axios.get(path);
        let data = [];
        for (let idx = 0; idx < res.data.data.length; idx++) {
          const tab = {
            title: res.data.data[idx].text,
            val: res.data.data[idx].id
          };
          data.push(tab);
        }
        this.faqTab = data;
      } catch (err) {
        console.log(err);
        this.faqTab = [];
      }
    },
    async getSupportAlert(key) {
      const path = `${ this.apiUrl }/api/page/bu/${ key }/services_alertinfo`;
      let res;
      try {
        res = await this.$axios.get(path);
        if (res.data.data.info.content) {
          this.servicesAlertMessage = res.data.data.info.content;
          this.$modal.show('v-modal');
        }
      } catch (err) {
        this.servicesAlertMessage = '';
      }
    },
    onResize() {
      if (process.client) {
        this.windowWidth = window.innerWidth;
      }
    },
  }
}

</script>

<style lang="scss">
  #supportBu{
    .default-tabs {
      padding-right: 0;
      .default-tabs__item + .default-tabs__item{
        word-wrap: break-word;
        margin-left: 1.389vw;
      }
    }
  }
</style>

<style lang="scss" scoped>

section.dropdown-container {
  display: none;
}

div.support-bu {
  padding-left: 7.3vw;
  width: 85.4vw;

  div.support-container {
    width: 100%;
  }
}

.pc-show{
  display: block;
}

.mobile-show{
  display: none;
}

@media screen and (max-width: 1024px) {
  div.support-bu {

    ul.breadcrumb {
      height: 6vw;
      width: 70vw;
      font-size: 3.200vw;
      margin-bottom: 3.200vw;
      margin-top: 10.400vw;
      line-height: 1.75;
    }

    .tab-container {
      display: none;
    }
  }

  section.dropdown-container {
    display: block;
  }

  div.support-container {
    width: 100%;
    margin-top: 13.227vw;
  }

  .pc-show{
    display: none;
  }

  .mobile-show{
    display: block;
  }

}
</style>
