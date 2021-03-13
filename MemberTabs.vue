<template>
  <div class="member-index">
    <ul class="breadcrumb" v-if="windowWidth <= xs">
      <li class="index">
        <a class="breadcrumb-link" href="/member/?tab=profile">{{ this.$t('memberCenter.title.memberCenter') }}</a>
      </li>
      <li class="current">
        <span>｜</span><a class="breadcrumb-link">{{ currentMemSelect }}</a>
      </li>
    </ul>
    <!-- PC view -->
    <section class="tab-container" v-if="!(windowWidth <= xs)">
      <tab
        :tabs="tabs"
        :current-tab="currentTabName" 
        :handle-click="handleClick"
        :content-padding-top="5.313"
        :styleObject="{
          width: '85.4vw',
          padding: '4.792vw 0vw 1.229vw 0'
        }" >
        <template v-slot:profile>
          <profile />
        </template>
        <template v-slot:psw-change>
          <psw-change />
        </template>
        <template v-slot:product-register>
          <product-register />
        </template>        
        <template v-slot:history-search>
          <product-searched-history />
        </template>
        <template v-slot:acronis>
          <acronis-data />
        </template>
        <template v-slot:online-repair>
          <!-- <online-repair /> -->
        </template>
      </tab> 
    </section>

    <!-- Mobile view -->
    <section class="dropdown-container" v-else>

      <dropdown-tabs
        :currentTab='currentTabName'
        :isOpen="isMobileTabsMenuOpen"
        :tabs="tabs"
        @handleClick="handleClick">
      </dropdown-tabs>

      <div class="member-container" v-if="currentTabName === 'profile' && windowWidth <= xs">
        <profile />
      </div>
      <div class="member-container" v-if="currentTabName === 'psw-change' && windowWidth <= xs">
        <psw-change />
      </div>
      <div class="member-container" v-if="currentTabName === 'product-register' && windowWidth <= xs">
        <product-register />
      </div>
      <div class="member-container" v-if="currentTabName === 'history-search' && windowWidth <= xs">
        <product-searched-history />
      </div>
      <div class="member-container" v-if="currentTabName === 'acronis' && windowWidth <= xs">
        <acronis-data />
      </div>
      <div class="member-container" v-if="currentTabName === 'online-repair' && windowWidth <= xs">
        <online-repair />
      </div>

    </section>

  </div>
</template>

<script>
import AcronisData from '@/components/member/AcronisData';
import cookies from 'js-cookie';
import DropdownTabs from '@/components/shared/DropdownTabs.vue';
import OnlineRepair from '@/components/member/OnlineRepair';
import ProductRegister from '@/components/member/ProductRegister';
import ProductSearchedHistory from '@/components/member/ProductSearchedHistory';
import Profile from '@/components/member/Profile';
import PswChange from '@/components/member/PswChange';
import Tab from '@/components/shared/Tabs';
import { mapState, mapActions } from 'vuex';

export default {
  name: 'MemberTabs',
  props:{
    currentTab: {
      type: String,
      default: 'profile'
    }
  },
  components: {
    AcronisData,
    DropdownTabs,
    OnlineRepair,
    Profile,
    PswChange,
    ProductRegister,
    Tab
  },
  data () {
    return {
      currentTabName:'',
      isMobileTabsMenuOpen: false,
      lastScrollY: 0,
      windowWidth: 0
    }
  },
  created() {
    this.checkExpires();
    this.currentTabName = this.currentTab || 'profile';
    this.onResize();
  },
  mounted() {
    this.triggerToStartScrollAnimation();
    this.$nextTick(() => {
      window.addEventListener('resize', this.onResize);
    });
  },
  beforeDestroy() { 
    window.removeEventListener('resize', this.onResize); 
  },
  computed: {
    ...mapState(['authToken', 'locale', 'xs', 'account']),
    currentMemSelect() {
      let currentMemSelect = this.tabs.find(e => {
        if(e.value === this.currentTabName) return e;
      })

      return currentMemSelect.title
    },
    tabs() {
      let tabs = [
        {
          title: this.$t('memberCenter.main.personalInfo'),
          value: 'profile'
        },
        {
          title: this.$t('memberCenter.main.changePassword'),
          value: 'psw-change'
        },
        {
          title: this.$t('memberCenter.main.productRegistration'),
          value: 'product-register'
        }, 
        {
          title: this.$t('memberCenter.main.history'),
          value: 'history-search'
        }, 
        {
          title: this.$t('memberCenter.main.acronisInfo'),
          value: 'acronis'
        }
      ]
      if(this.locale === 'tw'){
        tabs.push({
          title: this.$t('memberCenter.main.submitRmaRequest'),
          value: 'online-repair'
        })
      }
      return tabs;
    }
  },
  methods: {
    ...mapActions(['a_logout']),
    async getProfile() {
      this.$axios.setHeader('Authorization', `Bearer ${cookies.get('_adata_web')}`);
      this.$axios.setHeader('X-localization', this.locale === 'en' ? 'us' : this.locale);
      const path = `${ process.env.API_URL }/api/member/profile`;
      let res;
      try{
        res = await this.$axios.get(path);
        return;
      } catch (err) {
        if (err.response.status === 401) {
          //this.a_logout();
          //this.$router.push({ name: 'lang-member-login' });
        }
      }
    },
    onResize() {
      if (process.client) {
        this.windowWidth = window.innerWidth;
      }
    },
    checkExpires() {
      // this.getProfile();
      if (this.authToken) {
        this.getProfile();
      } else {
        //this.a_logout();
        //this.$router.push({ name: 'lang-member-login' });
      }
    },
    handleClick(newTab) {
      this.isMobileTabsMenuOpen = this.isMobileTabsMenuOpen ? false : true;
      this.$router.push({
        name: 'lang-member',
        query: {
          tab: newTab
        }
      });
       if (newTab === 'online-repair') {        
        window.open(`http://corp.adata.com/${this.locale}/onlinefix?email=${this.account}`);
      } else {        
        this.$router.push({
          name: 'lang-member',
          query: {
            tab: newTab
          }
        });
      } 
      this.currentTabName = newTab;
    },
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
    }
  }
}
</script>

<style lang="scss" scoped>

$member-green: #50cc76;

.bg-green{
  background-color: $member-green;
}

.member-index{
  margin-bottom: 9.479vw;
}

button{
  font-size: 1.250vw;
  width: 8.958vw;
  height: 2.917vw;
  color: #000;
  background: #f3f3f3;
  &.bg-green{
    color: #fff;
    &:hover{
        background: darken($member-green, 10%);
    }
  }
  &:hover{
    background: darken(#f3f3f3, 10%);
  }
}

section.tab-container {
  display: block;
}

section.dropdown-container {
  position: relative;
  display: none;
}

/* RWD */
@media screen and (max-width: 1024px) {

  .member-index{
    margin-bottom: 3vw;
  }

  section.tab-container {
    display: none;
  }
  
  section.dropdown-container {
    display: block;
  }

  div.member-container {
    width: 85vw;
    margin-top: 12.160vw;
  }
}

</style>

