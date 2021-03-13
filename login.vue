<template>
  <div class="login">
    <div class="login-frame">
      <h2>{{ this.$t('landingPage.main1.adataAccount') }}</h2>
      <div class="input-box">
        <input type="text" name="" id="acct" :placeholder="this.$t('signUpMember.main.email')" v-model="updateLoginContent.email">
        <input type="password" name="" id="psw" :placeholder="this.$t('signUpMember.main.enterPassword')" v-model="updateLoginContent.password">
        <div class="forgot-psw-box">
          <input 
          type="text"
          name="captcha"
          id="captcha"
          placeholder=""
          class="CAPTCHA"
          maxlength="4" size="4"
          v-model.lazy.trim="updateLoginContent.captcha"
          >
          <div class="captcha" style="width: 180px; height: 30px;">
            <s-identify :identifyCode="identifyCode"></s-identify>
            <div style="height: 20px;" class="get-captcha" @click="refreshCode()">
              ⟳
            </div>
          </div>
          
        </div>
        <div class="forgot-psw-box">
          <div class="forgot-psw" @click.prevent="forgotPsw = !forgotPsw">
            <a href="#">{{ this.$t('landingPage.main2.forgotPassword') }}</a>
          </div>
        </div>
        
      </div> 
      <button class="member-btn bg-green login-btn" @click="login">{{ this.$t('landingPage.main3.login') }}</button>
      <nuxt-link class="member-btn" :to="`/${locale}/member/register`">{{ this.$t('landingPage.main3.createAccount') }}</nuxt-link>
      <!-- <div class="other-logins">
        <div class="other-login-btn fb" @click="loginWithFacebook()">
            <div class="login-icon"></div>
            <div class="login-name">
              {{ this.$t('landingPage.main4.facebookLogIn') }}
            </div>
        </div>
        <div class="other-login-btn gmail" @click="loginWithGoogle()">
            <div class="login-icon"></div>
            <div class="login-name">
              {{ this.$t('landingPage.main4.googleLogIn') }}
            </div>
        </div>
      </div> -->
      <div class="psw-reset-box" v-show="forgotPsw">
        <p>
          {{ this.$t('landingPageForgetPassword.main.content1') }}
        </p>
        <div class="psw-reset">
          <input
          type="email"
          name="reset-email"
          id="reset-email"
          :placeholder="this.$t('signUpMember.main.email')"
          v-model.lazy.trim="resetPswEmail"
          >
          <button class="member-btn bg-green" @click="resetPsw()">{{ this.$t('landingPageForgetPassword.main.resetPassword') }}</button>
        </div>
      </div>
    </div>
    <div class="member-introduction">
      <div v-if="locale !== 'tw'">
        <article>
          <b>{{ this.$t('landingPage.main5.content1') }}</b>
          <ul>
            <li v-for="(item, idx) in memIntro" :key="idx">
              <p>
                {{ item }}
              </p>
            </li>
          </ul>
        </article>
      </div>
      <div v-else>
        <article>
          <b>{{ this.$t('landingPage.main5.content1') }}</b>
          <p>
            感謝您購買ADATA產品! 加入會員，您可享有以下權益。
          </p>
          <ul>
            <li>
              <p>
              ・下載免費軟體與軟體更新通知
              </p>
            </li>
            <li>
              <p>
              ・完善的售後服務: 進行產品註冊、線上報修*、個人資料更新等
              </p>
            </li>
            <li>
              <p>
              ・獲取最新消息: 訂閱電子報包含新品上市、促銷資訊、會員專屬活動等
              </p>
            </li>
          </ul>
        </article>
        <div class="member-service">
            <article>
              *若您的產品需要進行維修服務，建議您依照下列方式進行以順利排除困難。
              <ul>
                <li>
                  <p>
                    （1）可透過<a href="/tw/support/consumer?tab=faqs" target="_blank">常見問題</a>的查詢，初步判斷與排除異常狀況。
                  </p>
                </li>
                <li>
                  <p>
                    （2）可前往<a href="/tw/support/consumer?tab=warranty" target="_blank">產品保期查詢</a>，並確認<a href="/tw/support/consumer?tab=warranty&warranty=warrantyService" target="_blank">產品保固服務</a>。
                  </p>
                </li>
                <li>
                  <p>
                    （3）可至<a href="/tw/support/consumer?tab=services" target="_blank">全台威剛授權服務中心</a>，做記憶體現場換貨(依現場庫存狀況為準)或其他產品代收送維修之服務。
                  </p>
                </li>
                <li>
                  <p>
                    （4）可送回原來購買地點，由經銷商協助提供代送維修之服務。
                  </p>
                </li>
                <li>
                  <p>
                    （5）可選擇登入會員申請線上報修
                  </p>
                </li>
              </ul>                
            </article>
            <article>
              <p>
                完成申請後，請依照系統指示將產品妥善包裝並寄回指定地址。待威剛收到後將盡速為您進行檢修流程。
              </p>
            </article>
            <article>
              <p>
                消費者服務專線：0800-309-309
                <br />
                服務時間：星期一至五　09:00 ~ 12:00、13:00 ~ 18:00
                <br />
                非上班時間，請使用<a href="https://corp.adata.com/tw/support/online" target="_blank">聯絡我們</a>留言給客服人員，謝謝。
                <!-- 非上班時間，請使用<a href="/tw/contact" target="_blank">聯絡我們</a>留言給客服人員，謝謝。 -->
              </p>
            </article>
        </div>
      </div>
    </div>
    <modal-component :width='54.688' :height='20' closeBtn class="pc-show">
      <!-- <div class="modal-confirm">{{ this.$t('landingPageForgetPassword.popUp.content1') }}</div> -->
      <article class="requirement">
        <ul class="announce" :class="{ 'show' : announce.length }">
          <li v-for="(item, idx) in announce" :key="idx">
            <p>{{ item }}</p>
          </li>
        </ul>
      </article>  
    </modal-component>
    <modal-component :height="10" closeBtn class="mobile-show">
      <article class="requirement" v-if="announce.length">
        <ul>
          <li v-for="(item, idx) in announce" :key="idx">
            <p>{{ item }}</p>
          </li>
        </ul>
      </article>
    </modal-component>
  </div>
</template>

<script>

import cookies from 'js-cookie';
import ModalComponent from '~/components/shared/ModalComponent';
import SIdentify from "@/components/shared/SIdentify";
import { mapState, mapActions } from 'vuex';
import { FACEBOOK_APP_ID } from '@/plugins/const.js';

export default {
    name: 'Login',
    components:{
      ModalComponent,
      SIdentify
    },
    data () {
      return {
        announce: [],
        forgotPsw: false,
        resetPswEmail: '',
        identifyCode: "",
        identifyCodes: "0123456789abcdwerwshdjeJKDHRJHKOOPLMKQ",
        updateLoginContent: {
          email: '',
          password: '',
          captcha: ''
        }
      }
    },
    mounted() {
      this.facebookSDKSettings();
      if (cookies.get('_adata_web')) {
        this.$router.push({
          name: 'lang-member'
        });
      } else {
        if (navigator.userAgent.indexOf('MSIE') !== -1 || navigator.appVersion.indexOf('Trident/') > 0) {
         // MSIE
         this.isIE = true;
        }
        this.refreshCode();
      }
      this.triggerToStartScrollAnimation();
    },
    beforeDestroy() {
      window.removeEventListener('scroll', this.scrollAnimationController);
    },
    computed: {
      ...mapState(['authToken', 'locale']),
      acctPsw() {
        let acctPsw = {
          email: this.updateLoginContent.email,
          password: this.updateLoginContent.password
        };
        return acctPsw;
      },
      memIntro() {
        let intro = this.$t('landingPage.main5.content2');
        const sentence = intro.split('\n');
        return sentence;
      }
    },
    methods: {
      ...mapActions(['a_setToken', 'a_setAcct']),
      /**
       * [ Trigger to start scroll animation function ]
       */
      triggerToStartScrollAnimation() {
        this.$bus.$emit('setNavbarStatus', {
          status: 'up'
        });
        window.addEventListener('scroll', this.scrollAnimationController);
      },
      /**
       * [ scrollAnimationController function ]
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
      refreshCode() {
        this.identifyCode = "";
        this.makeCode(this.identifyCodes, 4);
      },
      randomNum(min, max) {
        max = max + 1
        return Math.floor(Math.random() * (max - min) + min)
      },
      // 隨機生成驗證碼字符串
      makeCode(data, len) {
        for (let i = 0; i < len; i++) {
          this.identifyCode += data[this.randomNum(0, data.length - 1)]
        }
      },
      checkform() {
        this.announce = [];

        if (!this.acctPsw.email || !this.acctPsw.password) {
          let log = this.locale === 'tw' ? '請輸入帳號密碼'
          : this.locale === 'cn' ? '请输入帐号密码'
          : this.locale === 'hk' ? '請輸入帳號密碼'
          : 'Please enter your E-mail(ID) / password.'
          this.announce.push(log);
        } else if (this.updateLoginContent.captcha !== this.identifyCode) {
          this.announce.push(this.$t('contactPage.verify.captcha'));
          this.updateLoginContent.captcha = '';
          this.refreshCode();
        }
        
        if (!this.announce.length) {
          return true;
        } else {
          this.show();
        }

      },
      /**
       * [ facebook SDK initialize ]
       */
      facebookSDKSettings() {
        window.fbAsyncInit = function () {
          FB.init({
            appId: FACEBOOK_APP_ID,
            cookie: true,
            xfbml: true,
            version: 'v9.0',
          });
          FB.AppEvents.logPageView();
        };

        (function (d, s, id) {
          let js, fjs = d.getElementsByTagName(s)[0];
          if (d.getElementById(id)) { return; }
          js = d.createElement(s); js.id = id;
          js.src = 'https://connect.facebook.net/en_US/sdk.js';
          fjs.parentNode.insertBefore(js, fjs);
        }(document, 'script', 'facebook-jssdk'));
      },
      /**
       * [ loginWithFacebook ]
       */
      async loginWithFacebook() {
        const _self = this;
        FB.login(async function (response) {
          if (response.status === 'connected') {
            const fbAccessToken = response.authResponse.accessToken;
            _self.loginWithSocialApp('facebook', fbAccessToken);
          } else {
            console.log(response);
          }
        }, {scope: 'public_profile, email'});
      },
      /**
       * [ loginWithGoogle ]
       */
      async loginWithGoogle() {
        this.announce = [];
        let googleUser;
        try {
          googleUser = await this.$gAuth.signIn();
          const googleToken = googleUser.getAuthResponse();
          this.loginWithSocialApp('google', googleToken.access_token);
        } catch (err) {
          this.announce.push(`取得 Google 授權失敗，請稍後再試`);
          this.show();
        }
      },
      /**
       * [loginWithSocialApp description]
       * @param  { String } type  [ Oauth Type ]
       * @param  { String } token [ Access Token ]
       */
      async loginWithSocialApp(type, token) {
        this.$axios.setHeader('X-localization', this.locale === 'en' ? 'us' : this.locale);
        const path = `${ process.env.API_URL }/api/member/social/login`;
        const body = {
          type: type,
          accessToken: token
        }
        let response;
        try {
          response = await this.$axios.post(path, body);
          if (response.data.status === "success") {
            this.announce = '';
          }
          let accessToken = response.data.data.access_token;
          let expiresIn = response.data.data.expires_in / (60 * 60 * 24);
          this.a_setToken( { accessToken } );
          cookies.set('_adata_web', accessToken, { expires: expiresIn });
          if (cookies.get('_adata_web')) {
            this.$router.push({
              path: '/member?tab=profile',
              query: {
                tab: 'profile'
              }
            });
          }
        } catch (err) {
          this.announce.push(`取得${ type }授權失敗，請稍後再試`);
          this.show();
        }
      },
      async getToken() {
        this.$axios.setHeader('X-localization', this.locale === 'en' ? 'us' : this.locale);
        const path = `${ process.env.API_URL }/api/member/login`;
        let response;
        try {
          response = await this.$axios.post(path, this.acctPsw);
          if (response.data.status === "success") {
            this.announce = '';
          }
          let accessToken = response.data.data.access_token;
          let expiresIn = response.data.data.expires_in / (60 * 60 * 24);
          this.a_setToken( { accessToken } );
          this.a_setAcct( { email: this.acctPsw.email } );
          cookies.set('_adata_web', accessToken, { expires: expiresIn });
          if (cookies.get('_adata_web')) {
            this.$router.push({
              path: `/${this.locale}/member?tab=profile`,
              query: {
                tab: 'profile'
              }
            });
          }
        } catch (err) {
          if (err.response.data.message === 'Account or password not match.') {
            let log = this.locale === 'tw' ? '帳號或密碼錯誤'
            : this.locale === 'cn' ? '帐号或密码错误'
            : this.locale === 'hk' ? '帳號或密碼錯誤'
            : 'The email address or password is incorrect.'
            this.announce.push(log);
            this.show();         
          } else if (err.response.data.message === 'The email address has not been verified.') {
            this.announce.push("該帳號信箱未驗證無法登入。");
            this.show();
          }
          let object = this.updateLoginContent;
          for (const key in object) {
            if (object.hasOwnProperty(key)) {
              object[key] = '';
            }
          }
          this.refreshCode();
        }
      },
      login() {
        if (this.checkform()) {
          this.getToken();
        }
      },
      show() {
        this.$modal.show('v-modal');
      },
      async resetPsw() {
        this.announce = [];
        this.$axios.setHeader('X-localization', this.locale === 'en' ? 'us' : this.locale);
        const path = `${ process.env.API_URL }/api/member/password/forgot`;
        let res;
        try {
          res = await this.$axios.post(path, { "email" : this.resetPswEmail });
          if (res.data.status === 'success') {
            // this.announce.push('密碼重設連結已寄送至您填寫的信箱中，請依照信件中的連結重設密碼。')
            this.announce.push(this.$t('landingPageForgetPassword.popUp.content1'))
            this.show();
            this.resetPswEmail = '';
          }
        } catch(err) {
          if (err.response.status === 422) {
            this.announce.push(this.$t('landingPageForgetPassword.popUp.content2'))
            this.show();
          }         
        }
      }
    }
}
</script>

<style lang="scss" scoped>

$member-green: #50cc76;

@mixin input-style($width, $height) {
  width: $width;
  height: $height;
  border: .3px solid #707070;
  padding: 0.417vw 0.938vw;
  box-sizing: border-box;
  font-size: 1.042vw;
  line-height: 2.396vw; 
  & + input {
      margin-top: 0.990vw;
  }
}

.bg-green {
  background-color: $member-green;
}

 h2 {
  font-size: 2.240vw;
  line-height: 1.911vw;
  text-align: center;
}

.login {
  width: 100%;
  box-sizing: border-box;
  display: flex;
  justify-content: center;
  align-items: center;
  padding-top: 4.823vw;
  padding-bottom: 4.323vw;
  height: auto;
  .login-frame {
    display: flex;
    justify-content: center;
    align-items: center;
    align-content: center;
    flex-wrap: wrap;
    width: 36.6%;
    padding: 0 8.938vw 0 0;
    box-sizing: border-box;       
    .input-box {
      margin-top: 1.615vw;
      margin-bottom: 0.990vw;
      input {
        @include input-style(100%, 2.292vw)
      }
      .forgot-psw-box {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-top: 0.990vw;
        input {
          width: 9.375vw;
        }
        .forgot-psw a {
          width: 3.646vw;
          color: #000;
          font-size: 0.885vw;
          text-decoration: underline;
        }
      }
    }
    .announce {
      visibility: hidden;
      opacity: 0;
      height: 0;
      text-align: left;
      // color: #fd345c;
      width: 100%;
      transition: visibility .4s, opacity .5s linear;
      &.show {
        visibility: visible;
        opacity: 1;
        height: auto;
        padding: 0 0 10px;
      }
    }
    .member-btn {
        width: 48.5%;
    }
    .other-logins {
      width: 100%;
      display: flex;
      justify-content: center;
      align-content: center;
      margin-top: 2.219vw;
      .other-login-btn {
        width: 50%;
        height: auto;
        display: flex;
        justify-content: center;
        align-content: center;
        cursor: pointer;
        transition: all .8s;
        &:hover {
            opacity: .6;
        }
        .login-icon {
          width: 2.063vw;
          height: 2.063vw;
          border-radius: 50%;
          margin-right: 0.958vw;
        }
        .login-name {
          line-height: 2.063vw;
        }
        &.fb {
          border-right: solid 0.3px #000000;
          .login-icon {
            background:#4066B1 url('~assets/img/member/facebook-icon.png') center/contain no-repeat;
          }
        }
        &.gmail {
          .login-icon {
            background:#EB5142 url('~assets/img/member/google-icon.png') center/contain no-repeat;
          }
        }
      }
    }
    .psw-reset-box {
        margin-top: 1.802vw;
        .psw-reset {
          display: flex;
          justify-content: space-between;
          margin-top: 0.521vw;

          input {
            @include input-style(auto, 2.292vw);
            width: calc(98% - 5.573vw);
          }

          .member-btn {
            font-size: 1.042vw;
            width: 5.573vw;
            height: 2.292vw;
            padding-left: 0;
            padding-right: 0;
          }
        }
    }
  }
  .member-introduction {
    // width: calc(100% - 27.917vw);
    padding-left: 6.073vw;
    box-sizing: border-box;
    border-left: 0.3px solid #b0b0b0;
    width: 51.911vw;

    article {
      margin-bottom: 1.146vw;
    }
    .member-service {

      article {
        font-size: 0.885vw;
        line-height: 1.354vw;
        a {
          font-size: 0.885vw;
          color: $member-green;
          text-decoration: underline;
        }

      }
    }
  }
}

/* RWD */
@media screen and (max-width: 1366px) {
  .login {

    .login-frame {
      width: 37vw;
    }

  }
}

@media screen and (max-width: 1024px) {
  h2 {
    font-size: revert;
    line-height: 1;
    margin-bottom: 5.067vw;
  }

  .login {
    flex-direction: column;
    .login-frame {
      width: 100%;
      padding: 10.400vw 0 19.467vw;
      flex-direction: column;
      .input-box {
        width: 100%;
        input {
          line-height: 2.1;
          padding: 4.5vw 4vw;
          font-size: 4vw;
          & + input {
              margin-top: 3.467vw;
          }
        }
        .forgot-psw-box {
          margin-top: 3.467vw;
          input {
            width: 30vw;
          }          
          .forgot-psw a {
            font-size: 4vw;
          }
        }
      }
      .other-logins {
        margin-top: 4vw;
        order: 0;
        .other-login-btn{
          .login-icon{
            width: 7.413vw;
            height: 7.413vw;
            margin-right: 2.027vw;
          }
          .login-name {
            font-size: 4vw;
            line-height: 7.413vw;
          }
        }
      }
      .psw-reset-box {
        line-height: 1.7;
        padding: 5.067vw 0 0;
        margin-top: 0;
        order: 2;
        .psw-reset {
          flex-direction: column;
          justify-content: center;
          input {
            font-size: 4vw;
            width: 100%;
            line-height: 2.1;
            padding: 4.5vw 4vw;
          }
          .member-btn {
            font-size: 4vw;
            width: 52vw;
            height: auto;
            margin: 3.467vw auto 0;
          }
        }
      }
      .member-btn {
        width: 52vw;        
        order: 1;
        &.login-btn {
          margin-top: 9.600vw;
          margin-bottom: 1.333vw;
        }
      }
    }
    .member-introduction {
      width: 100%;
      border-left-width: 0;
      padding: 0;      
      article {
        margin-bottom: 5.600vw;
        p {
          font-size: 4vw;
          line-height: 1.4;
        }
      }
      .member-service {
        article {
          line-height: 1.4;
          font-size: 4vw;
          a {
            font-size: 4vw;
          }
        }
      }
    }
  }
}

</style>
