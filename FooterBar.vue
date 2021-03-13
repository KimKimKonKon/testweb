<template>
  <footer id="footerbar" class="footer-frame">
    <div class="pc-view" :class="{ dark }">
      <div class="back-top-container" :style="{ borderColor: dark ? 'white' : '#b0b0b0'  }">
        <div class="back-top">
          <div class="arrow-top">
            <img src="~/assets/img/overall/arrow-top.svg" v-if="!dark" style="width: 1.1vw; height: 0.9vw;">
            <img src="~/assets/img/overall/arrow-top-2.svg" v-else style="width: 1.1vw; height: 0.9vw;">
          </div>
          <client-only>
            <VueElevator v-show="currentPage !== 'lang-product-productPage' " class="back-top-btn" :word="$t('element.main.content')" :duration="300" mainAudio="null" endAudio="null">
            </VueElevator>
            <div v-show="currentPage === 'lang-product-productPage' " class="back-top-btn-product" @click="emitScrollEvent()">{{ $t('element.main.content') }}</div>
          </client-only>
        </div>
      </div>
      <div class="footer-container">
        <language-switch></language-switch>
        <div class="site-map">  
          <div
            v-for="(footerItem, idx) in footerList"
            :key="idx"
            class="footer-item item-hover"
          >
            <span @click="routeToPage(footerItem.pageName)">{{ footerItem.title }}</span>
            <hr />
          </div> 
        </div>    
        <div class="footer-tools">
          <ul class="tool-items">
            <!--  第四階段先隱藏
            <li>
              <nuxt-link :to='`/${locale}/terms`' target="_blank">  
                {{ TermsOfUse }}
              </nuxt-link>
            </li>
            <li>
              <nuxt-link :to='`/${locale}/privacy`' target="_blank">
                {{ privacy }}
              </nuxt-link>
            </li> -->
            <li>
              <a :href="`https://corp.adata.com/${ locale }/about/term/`" target="_blank">
                {{ TermsOfUse }}
              </a>
            </li>
            <li>
              <a :href="`https://corp.adata.com/${ locale }/about/privacy/`" target="_blank">
                {{ privacy }}
              </a>
            </li>
          </ul>
          <!-- Gerenal -->
          <ul class="social-items" v-if="!isXPG">
            <li
              v-for="(item, idx) in socialMediaList"
              :key="idx"
              v-show="item.link"
            >
              <a :href="item.link" target="_blank" :title="item.pageName">
                <img :src="!dark ? item.imgUrl : item.imgUrlWhite " alt="">
              </a>
            </li>
          </ul>
          <!-- XPG -->
          <ul class="social-items" v-else>
            <li
              v-for="(item, idx) in xpgSocialMediaList"
              :key="idx"
              v-show="item.link"
            >
              <a :href="item.link" target="_blank" :title="item.pageName">
                <img :src="!dark ? item.imgUrl : item.imgUrlWhite " alt="">
              </a>
            </li>
          </ul>
        </div>
      </div>
    </div>

    <div class="mobile-view dark">
      <div class="m-footer-container">
        <div class="m-site-map">
          <div
            v-for="(footerItem, idx) in mobileFooterList"
            :key="idx"
            class="footer-item item-hover"
            @click="routeToPage(footerItem.pageName)"
          >
            <span>{{ footerItem.title }}</span>
            <br />
          </div>
        </div>
        <div class="m-select-container">
        </div>
      </div>
      <div class="selected-lang" :style="{ paddingBottom: mobileLanguage === '22vw' ? '4vw' : '0vw', borderBottom: mobileLanguage === '22vw' ? '1px solid #fff' : '0px' }">
        <br />
          <div @click="openLanguage()">
            <language-switch></language-switch>
          </div>
      </div>

      <div class="m-footer-tools">
        <ul class="term-of-use">
          <!-- 第四階段先隱藏
          <li>
            <nuxt-link :to='`/${locale}/terms`' target="_blank" title="Term_of_Use">   
              {{ $t('footer.TermsOfUse.title') }}
            </nuxt-link>
          </li>
          <li>
            <nuxt-link :to='`/${locale}/privacy`' target="_blank" title="privacy">
              {{ $t('footer.privacy.title') }}
            </nuxt-link>
          </li> -->
          <li>
            <a :href="`https://corp.adata.com/${ locale }/about/term/`" target="_blank" title="Term_of_Use">
              {{ $t('footer.TermsOfUse.title') }}
            </a>
          </li>
          <li>
            <a :href="`https://corp.adata.com/${ locale }/about/privacy/`" target="_blank" title="privacy">
              {{ $t('footer.privacy.title') }}
            </a>
          </li>
        </ul>
        <!-- Gerenal -->
        <ul class="m-social-items" v-if="!isXPG">
          <li
            v-for="(item, idx) in socialMediaList"
            :key="idx"
            v-show="item.link"
          >
            <a :href="item.link" target="_blank" :title="item.pageName">
              <img :src="item.imgUrlWhite " alt="">
            </a>
          </li>
        </ul>
        <!-- XPG -->
        <ul class="m-social-items" v-else>
          <li
            v-for="(item, idx) in xpgSocialMediaList"
            :key="idx"
            v-show="item.link"
          >
            <a :href="item.link" target="_blank" :title="item.pageName">
              <img :src="item.imgUrlWhite " alt="">
            </a>
          </li>
        </ul>
        <!-- <ul class="m-social-items">
          <li>
            <a href="https://www.facebook.com/adatatw" target="_blank" title="facebook">
              <img src="~/assets/img/overall/icon-fb-02-2@3x.png" alt="">
            </a>
          </li>
          <li>
            <a href="https://www.instagram.com/adata_global/?hl=zh-tw" target="_blank" title="instagram">
              <img src="~/assets/img/overall/icon-ig-01-2@3x.png" alt="">
            </a>
          </li>
          <li>
            <a href="https://tw.linkedin.com/company/adata-technology" title="LinkedIn" target="_blank">
              <img src="~/assets/img/overall/icon-li-05-2@3x.png" alt="">
            </a>
          </li>
          <li>
            <a href="https://twitter.com/adatatechnology" target="_blank" title="twitter">
              <img src="~/assets/img/overall/icon-tw-03-2@3x.png" alt="">
            </a>
          </li>
          <li>
            <a href="https://www.youtube.com/user/ADATAGroup" target="_blank" title="youtube">
              <img src="~/assets/img/overall/icon-yt-04-2@3x.png" alt="">
            </a>
          </li>        
        </ul> -->
      </div>
    </div>
    <LoadingBar :activate="setLoading" />
  </footer>
</template>

<script>
import OpenIndicator from '@/components/layout/footer/OpenIndicator';
import LanguageSwitch from '@/components/layout/footer/LanguageSwitch'
import LoadingBar from '@/components/shared/LoadingBar'
import vSelect from 'vue-select';
import { VueElevator } from 'vue-elevator';
import { mapState } from 'vuex';

vSelect.props.components.default = () => ({ OpenIndicator });

export default {
  name: 'Footerbar',
  props: {
    dark: {
      type: Boolean,
      default: false
    },
    dataFooter: {
      type: Object,
      default: () => {}
    }
  },
  components: {
    'v-select': vSelect,
    VueElevator,
    LanguageSwitch,
    LoadingBar
  },
  data () {
    return {
      currentPage: '',
      mobileLanguage: '0vw',
      openLanguageMenu: false,
      options: [
        'ENGLISH ｜ US',
        '繁體中文 ｜ TW'
      ],
      selected: 'ENGLISH ｜ US',
      TermsOfUse: this.dataFooter.data ? this.dataFooter.data.data.footer_disclaimer.filter(e=>e.key === 'terms_of_use')[0].val : this.$t('footer.TermsOfUse.title'),
      privacy: this.dataFooter.data ? this.dataFooter.data.data.footer_disclaimer.filter(e=>e.key === 'privacy')[0].val : this.$t('footer.privacy.title'),
      isXPG: false,
      setLoading: {}
    };
  },
  created() {
    this.$bus.$on('actLoaing', this.activateLoading);
  },
  mounted() {
    this.currentPage = this.$route.name;
    if (this.$route.params.product === 'xpg') {
      this.isXPG = true;
    } else {
      this.isXPG = false;
    }
  },
  watch: {
    $route(newValue) {
      this.currentPage = newValue.name;
      const buSelect = newValue.params.product;
      if (buSelect === 'xpg') {
        this.isXPG = true;
      } else {
        this.isXPG = false;
      }
    },
    dataFooter(val) {
      this.TermsOfUse = this.dataFooter.data ? this.dataFooter.data.data.footer_disclaimer.filter(e=>e.key === 'terms_of_use')[0].val : this.$t('footer.TermsOfUse.title');
      this.privacy = this.dataFooter.data ? this.dataFooter.data.data.footer_disclaimer.filter(e=>e.key === 'privacy')[0].val : this.$t('footer.privacy.title');
    }
  },
  computed: {
    ...mapState(['locale']),
    footerList() {
      return [
        {
          title: this.dataFooter.data ? this.dataFooter.data.data.footer_menu.filter(e=>e.key === 'contact_and_inquiry')[0].val : this.$t('footer.contact.title'),
          // pageName: this.$t('footer.contact.link')
          //pageName: 'lang-contact'
            pageName: `https://corp.adata.com/${ this.locale }/support/online`
        },
        {
          title: this.dataFooter.data ? this.dataFooter.data.data.footer_menu.filter(e=>e.key === 'career')[0].val : this.$t('footer.career'),
          // pageName: 'lang-career'
          pageName: this.locale === 'tw' ? 'https://corp.adata.com/static/career/tw/career_tw.html' : 'https://corp.adata.com/static/career/career2.html'
        },
        {
          title: this.dataFooter.data ? this.dataFooter.data.data.footer_menu.filter(e=>e.key === 'investor_relations')[0].val : this.$t('footer.investor.title'),
          pageName: `https://corp.adata.com/${ this.locale }/about/investor`
          // pageName: 'lang-investor'
        },
        {
          title: this.dataFooter.data ? this.dataFooter.data.data.footer_menu.filter(e=>e.key === 'csr')[0].val : this.$t('footer.csr'),
          // pageName: 'lang-csr'
          pageName: this.locale === 'tw' ? 'https://corp.adata.com/static/csr/tw/CSR.html' : 'https://corp.adata.com/static/csr/CSR.html'
          
        },
        {
          title: this.dataFooter.data ? this.dataFooter.data.data.footer_menu.filter(e=>e.key === 'award')[0].val : this.$t('footer.award'),
          pageName: 'lang-award'
        },
        {
          title: this.dataFooter.data ? this.dataFooter.data.data.footer_menu.filter(e=>e.key === 'taiwan_sports_lottery ')[0].val : this.$t('footer.lottery'),
          pageName: this.locale === 'tw' ? 'https://corp.adata.com/tw/aa/newslist/5/' : 'https://corp.adata.com/en/aa/newslist/5/'
        }
      ];
    },
    mobileFooterList() {
      return [
        {
          title: this.dataFooter.data ? this.dataFooter.data.data.footer_menu.filter(e=>e.key === 'contact_and_inquiry')[0].val : this.$t('footer.contact.title'),
          // pageName: 'lang-contact'
          // pageName: this.$t('footer.contact.link')
            pageName: `https://corp.adata.com/${ this.locale }/support/online`
        },
        {
          title: this.dataFooter.data ? this.dataFooter.data.data.footer_menu.filter(e=>e.key === 'csr')[0].val : this.$t('footer.csr'),
          // pageName: 'lang-csr'
          pageName: this.locale === 'tw' ? 'https://corp.adata.com/static/csr/tw/CSR.html' : 'https://corp.adata.com/static/csr/CSR.html'
        },
        {
          title: this.dataFooter.data ? this.dataFooter.data.data.footer_menu.filter(e=>e.key === 'career')[0].val : this.$t('footer.career'),
          // pageName: 'lang-career'
          pageName: this.locale === 'tw' ? 'https://corp.adata.com/static/career/tw/career_tw.html' : 'https://corp.adata.com/static/career/career2.html'
        },
        {
          title: this.dataFooter.data ? this.dataFooter.data.data.footer_menu.filter(e=>e.key === 'award')[0].val : this.$t('footer.award'),
          pageName: 'lang-award'          
        },
        {
          title: this.dataFooter.data ? this.dataFooter.data.data.footer_menu.filter(e=>e.key === 'investor_relations')[0].val : this.$t('footer.investor.title'),
          // pageName: `https://corp.adata.com/${ this.locale }/about/investor`
          
          pageName: 'lang-investor'
        },
        {
          title: this.dataFooter.data ? this.dataFooter.data.data.footer_menu.filter(e=>e.key === 'taiwan_sports_lottery ')[0].val : this.$t('footer.lottery'),
          pageName: this.locale === 'tw' ? 'https://corp.adata.com/tw/aa/newslist/5/' : 'https://corp.adata.com/en/aa/newslist/5/'
        },
      ];
    },
    socialMediaList() {
      return [
        {
          link: this.dataFooter.data ? this.dataFooter.data.data.social_media.find(e=>e.key === 'facebook').val : null,
          pageName: this.dataFooter.data ? this.dataFooter.data.data.social_media.find(e=>e.key === 'facebook').key : null,
          imgUrl: require('~/assets/img/overall/icon-fb-02@3x.png'),
          imgUrlWhite: require('~/assets/img/overall/icon-fb-02-2@3x.png')
        },
        {
          link: this.dataFooter.data ? this.dataFooter.data.data.social_media.find(e=>e.key === 'instagram').val : null,
          pageName: this.dataFooter.data ? this.dataFooter.data.data.social_media.find(e=>e.key === 'instagram').key : null,
          imgUrl: require('~/assets/img/overall/icon-ig-01@3x.png'),
          imgUrlWhite: require('~/assets/img/overall/icon-ig-01-2@3x.png')
          
        },
        {
          link: this.dataFooter.data ? this.dataFooter.data.data.social_media.find(e=>e.key === 'linkedIn').val : null,
          pageName: this.dataFooter.data ? this.dataFooter.data.data.social_media.find(e=>e.key === 'linkedIn').key : null,
          imgUrl: require('~/assets/img/overall/icon-li-05@3x.png'),
          imgUrlWhite: require('~/assets/img/overall/icon-li-05-2@3x.png')
        },
        {
          link: this.dataFooter.data ? this.dataFooter.data.data.social_media.find(e=>e.key === 'twitter').val : null,
          pageName: this.dataFooter.data ? this.dataFooter.data.data.social_media.find(e=>e.key === 'twitter').key : null,
          imgUrl: require('~/assets/img/overall/icon-tw-03@3x.png'),
          imgUrlWhite: require('~/assets/img/overall/icon-tw-03-2@3x.png')
        },
        {
          link: this.dataFooter.data ? this.dataFooter.data.data.social_media.find(e=>e.key === 'youtube').val : null,
          pageName: this.dataFooter.data ? this.dataFooter.data.data.social_media.find(e=>e.key === 'youtube').key : null,
          imgUrl: require('~/assets/img/overall/icon-yt-04@3x.png'),
          imgUrlWhite: require('~/assets/img/overall/icon-yt-04-2@3x.png')
        }
      ]
    },
    xpgSocialMediaList() {
      return [
        {
          link: 'https://www.facebook.com/XPGglobal/',
          pageName: this.dataFooter.data ? this.dataFooter.data.data.social_media.find(e=>e.key === 'facebook').key : null,
          imgUrl: require('~/assets/img/overall/icon-fb-02@3x.png'),
          imgUrlWhite: require('~/assets/img/overall/icon-fb-02-2@3x.png')
        },
        {
          link: 'https://www.instagram.com/accounts/login/?next=/xpg_global/',
          pageName: this.dataFooter.data ? this.dataFooter.data.data.social_media.find(e=>e.key === 'instagram').key : null,
          imgUrl: require('~/assets/img/overall/icon-ig-01@3x.png'),
          imgUrlWhite: require('~/assets/img/overall/icon-ig-01-2@3x.png')
          
        },
        {
          link: this.dataFooter.data ? this.dataFooter.data.data.social_media.find(e=>e.key === 'linkedIn').val : null,
          pageName: this.dataFooter.data ? this.dataFooter.data.data.social_media.find(e=>e.key === 'linkedIn').key : null,
          imgUrl: require('~/assets/img/overall/icon-li-05@3x.png'),
          imgUrlWhite: require('~/assets/img/overall/icon-li-05-2@3x.png')
        },
        {
          link: 'https://twitter.com/XPG_Global',
          pageName: this.dataFooter.data ? this.dataFooter.data.data.social_media.find(e=>e.key === 'twitter').key : null,
          imgUrl: require('~/assets/img/overall/icon-tw-03@3x.png'),
          imgUrlWhite: require('~/assets/img/overall/icon-tw-03-2@3x.png')
        },
        {
          link: 'https://www.youtube.com/channel/UCsLsrE0vPP7EUs7Tza_lsTQ',
          pageName: this.dataFooter.data ? this.dataFooter.data.data.social_media.find(e=>e.key === 'youtube').key : null,
          imgUrl: require('~/assets/img/overall/icon-yt-04@3x.png'),
          imgUrlWhite: require('~/assets/img/overall/icon-yt-04-2@3x.png')
        }
      ]
    }

  },
  methods: {
    emitScrollEvent() {
      // window.scrollTo(0, 200);
      this.$bus.$emit('callScrollToTab', true);
    },
    openLanguage() {
      if (this.mobileLanguage === '22vw') {
        this.mobileLanguage = 'auto';
      } else {
        this.mobileLanguage = '22vw';
      }
    },
    routeToPage(pageName) {
      if (pageName.indexOf('lang-') < 0) {
        window.open(pageName, '_blank');
      } else {
        this.$router.push({
          name: pageName,
          params: {
            lang: this.locale
          }
        });
      }
    },
    activateLoading(e) {
      this.setLoading = e
    }
  }
};
</script>

<style lang="scss" >

$font-size : 1vw;

@mixin font-style($weight, $text-align:center) {
  font-weight: $weight;
  font-size: $font-size;
	font-stretch: normal;
	font-style: normal;
	line-height: 1.15;
	color: #000;
	text-align: $text-align;
	cursor: pointer;
}

.footer-frame {
  background-color: white;
  .pc-view{
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    align-items: flex-start;
    align-content: flex-start;
    // width: 100vw;
    // height: 13.5vw;    
    padding: 0 7.3vw;
  }
  .dark{
    background-color: black;
    *{
      color: white !important;      
    }
    svg{
      fill: white;
    }
  }
}

.footer-container {
  display: flex; 
  flex-wrap: wrap;
  justify-content: space-between;
  width: 100vw;
  height: auto;
  margin-bottom: 2vw;
}

.site-map {
  width: 47vw;
	display: flex;
	flex-wrap: wrap;
	justify-content: space-between;
	align-items: center;
  align-content:center;
}

.site-map > .footer-item {
  @include font-style(500);
}

/* back to Top */

.back-top-container {
  width: 100%;
  display: flex;
  justify-content: flex-end;
  align-items: center;
  border-bottom: 0.5px solid #b0b0b0;
  height: 4.9vw;
  margin-bottom: 1.5vw;

  .back-top {
    position: relative;
    width: 8.6vw;
    display: flex;
    justify-content: space-between;
    align-items: center;
    cursor: pointer;

    .back-top-btn {
    @include font-style(bold, left);
      width: 100%;
      z-index: 9;
      text-align: right;
      .elevator-button {
        padding-right: 1.8vw;
      }
    }

    .back-top-btn-product {
      @include font-style(bold, left);
      width: 100%;
      z-index: 9;
      text-align: left !important;
    }

    .arrow-top {
      position: absolute;
      right: 0;
      top: 0;
      img{
        width: 1.1vw;
        height: auto;
      }
    }
  }
}

.elevator-button{
  .sweet-svg{
    display: none;
  }
}

/* v-select */

.select-container{
  width: 100%;
  display: flex;
  align-items: center;
  margin-bottom: 0.9vw;
}

.v-select{
  &.customize{
    cursor: pointer;
    width: 10vw;
    height: 1.2vw;
    .vs__dropdown-toggle{
      height: 100%;
      padding: 0;
      border: none;
      border-radius: 0;
    }
    .vs__selected{
      font-size: $font-size;
      margin: 0;
      padding: 0;
      position: absolute;
    }
    .vs__selected-options{
      width: 7.6vw;
      padding: 0;
    }
    .vs__search{
      padding: 0;
    }
    .vs__actions{
      padding: 0;
    }
  }
}

/* #vs1__listbox 語言下拉選單的 id , prop: appendToBody */

#vs1__listbox {
  top: auto !important;
  bottom: calc(100% + 5px);
  // display: block !important;
  // visibility: visible !important;
  border-radius: 0;
  font-size: $font-size;
  font-family: Helvetica, '微軟正黑體';
  font-weight: bold;
  background-color: hsla(0,0%,100%,.7);
  filter: blur(0px);
  .vs__dropdown-option{  /* 每個選項的 class */
    &.vs__dropdown-option--selected{  /* 被選到的選項的 class */
      color:#043FC1 !important;
    }
    &.vs__dropdown-option--highlight{ /* 滑過的選項的 class */
      color:#043FC1 !important;
      background-color: transparent !important;
    }
  }
}

.customize.dark #vs1__listbox {  
  background-color: black;
  .vs__dropdown-option{  /* 每個選項的 class */
    &.vs__dropdown-option--selected{  /* 被選到的選項的 class */
      color: red !important;
    }
    &.vs__dropdown-option--highlight{ /* 滑過的選項的 class */
      color: red !important;
      background-color: transparent !important;
    }
  }
}

/* ↑ #vs1__listbox 語言下拉選單的 id , 在 body 裡面最外層 ↑ */

.global-btn{
  background-color: transparent;
  width: 1vw;
  height: 1vw;
  padding: 0;
  border:none;
  margin-left: 1vw;
  cursor: pointer;
}

/* right-side footer tools */

.footer-tools{
  display: flex;
  justify-content: flex-end;
  align-items: center;
  .tool-items, .social-items{
    display: flex;
    justify-content: space-between;
    @include font-style(normal);
    a{
      font-size: $font-size;
      text-decoration: none;
      color: #666;
    }
  }
  .tool-items{
    width: 13vw;
  }
  .social-items{
    width: 11.8vw;
    margin-left: 3vw;
    li{
      img{
        width: 1.6vw;
        height: 1.6vw;
      }
    }
  }
}

.item-hover {
  color: #666 !important;
  /*border-bottom: 0px solid #666;*/
  transition: 0.4s;
}

.item-hover > hr {
  opacity: 0;
  width: 0px;
  border: 1px solid #666;
  transition: 0.4s;
}

.item-hover:hover {
  color: #000 !important;
  /*border-bottom: 1px solid #000;*/
}

.item-hover:hover > hr {
  color: #000;
  text-shadow: 0 5px 10px #141414;
  opacity: 1;
  width: 100%;
}

.mobile-view{
  display: none;
}

/* RWD */

@media screen and (max-width: 1024px) {

  .footer-item {
    font-weight: 300;
    font-size: 4vw;
    font-stretch: normal;
    font-style: normal;
    line-height: 1.15;
    
    color: #000;
    cursor: pointer;
    width: 50%;
    margin-top: 2.6vw;
  }

  .selected-lang {
    height: auto;
    font-weight: 300;
    font-size: 4vw;
    font-stretch: normal;
    font-style: normal;
    line-height: 1.15;
    
    color: #000;
    width: 100%;  
    overflow: hidden;
    transition: 0.4s;
  }

  .arrow-language {
    margin-left: 1vw;
    height: 5vw;
  }

  .footer-frame{
    .pc-view{
      display: none;
    }
    .mobile-view{
      display: block;
      padding: 7.200vw 7.600vw ;
      height: auto;
      overflow: hidden;    
      .m-footer-container{
        width: 100%;
        height: 28vw;
        border-bottom: 1px solid #fff;
        font-weight: 300;
        font-size: 4vw;
        font-stretch: normal;
        font-style: normal;
        line-height: 1.15;
        
        color: #000;
        .m-site-map{
          display: flex;
          justify-content: space-around;
          flex-wrap: wrap;
          margin-bottom: 4.693vw;
          a{
            font-size: 4.05vw;

            display: block;
            & + a{
              margin-top: 2.400vw;
            }            
          }
        }
        .m-select-container{
          .v-select.customize{
            width: 39.5vw;
            .vs__actions{
              margin-top: 10px;
              svg{
                width: 4.747vw;
              }
            }
            .vs__selected-options{
              .vs__selected{
                font-size: 4.05vw;
              }
            }
            #vs2__listbox {
              bottom: auto !important;
              top: calc(100% + 20px);
              // display: block !important;
              // visibility: visible !important;
              border-radius: 0;
              font-size: 4.05vw;
              font-family: Helvetica, '微軟正黑體';
              font-weight: bold;
              background-color: hsla(0,0%,100%,.7);
              filter: blur(0px);
              .vs__dropdown-option{  /* 每個選項的 class */
                &.vs__dropdown-option--selected{  /* 被選到的選項的 class */
                  color:#043FC1 !important;
                }
                &.vs__dropdown-option--highlight{ /* 滑過的選項的 class */
                  color:#043FC1 !important;
                  background-color: transparent !important;
                }
              }
            }
            &.dark #vs2__listbox {
              background-color: black;
              .vs__dropdown-option{  /* 每個選項的 class */
                &.vs__dropdown-option--selected{  /* 被選到的選項的 class */
                  color: red !important;
                }
                &.vs__dropdown-option--highlight{ /* 滑過的選項的 class */
                  color: red !important;
                  background-color: transparent !important;
                }
              }
            }
          }
        }
      }
      .m-footer-tools{
        .term-of-use{
          padding: 4.693vw 0;
          line-height: 8vw;
          display: flex;
          justify-content: flex-start;
          li + li{
            margin-left: 3.200vw;
          }
          a{
            font-weight: 300;
            font-size: 4vw;
          }
        }
        .m-social-items{
          width: 100%;
          display: flex;
          justify-content: flex-start;
          font-weight: normal;
          font-size: 3.467vw;
          font-stretch: normal;
          font-style: normal;
          line-height: 1.15;
          
          li{
            font-size: 4.05vw;
            line-height: 8vw;
            margin-right: 5.067vw;
            img{
              width: 8vw;
              height: 8vw;
            }
          }
        }
      }
    } 
  }
}
</style>
