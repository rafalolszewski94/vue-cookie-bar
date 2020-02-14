<template>
  <div>
    <transition name="slide-out">
      <div class="cookie-bar" v-if="display">
        <div class="cookie-bar-wrapper" :class="{'cookie-bar-container': containered}"
             :style="containered ? {'max-width': `${containerWidth}px`} : {}">
          <span>{{text}}</span>
          <a :href="link" v-if="link" @click="customLink ? $emit('linkClick') : null">{{linkText}}</a>
          <button class="cookie-bar-button" @click="accept">{{acceptText || lang && lang.accept}}</button>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
  const languages = [
    'en',
    'pl'
  ];

  export default {
    props: {
      text: {
        type: String,
        default: () => 'Ta witryna korzysta z plików cookie, aby polepszyć doświadczenie przeglądania i zapewnić dodatkową funkcjonalność.'
      },
      acceptText: {
        type: String,
        default: () => ''
      },
      linkText: {
        type: String,
        default: () => 'Więcej informacji'
      },
      link: {
        type: String,
        default: () => ''
      },
      customLink: {
        type: Boolean,
        default: false
      },
      containered: {
        type: Boolean,
        default: false,
      },
      containerWidth: {
        type: Number,
        default: () => 1240,
      }
    },
    data() {
      return {
        display: true,
        lang: undefined
      }
    },
    watch: {
      cookies: {
        handler(cookies) {
          this.display = !cookies.cookie_bar_accecpted
        },
        immediate: true
      }
    },
    created() {
      let userLang;
      if (!userLang) {
        userLang = navigator.language || navigator.userLanguage;
      }
      userLang = userLang.substr(0, 2);
      if (languages.indexOf(userLang) < 0) {
        userLang = 'en';
      }
      import(/* webpackChunkName: "async-lang/[request]" *//* webpackMode: "eager" */ `@/languages/${userLang}.js`).then(({ default: lang }) => {
        this.lang = lang
      });
    },
    computed: {
      cookies() {
        return document.cookie
            .split(';')
            .reduce((res, c) => {
              const [key, val] = c.trim().split('=').map(decodeURIComponent);
              const allNumbers = str => /^\d+$/.test(str);
              try {
                return Object.assign(res, { [key]: allNumbers(val) ? val : JSON.parse(val) })
              } catch (e) {
                return Object.assign(res, { [key]: val })
              }
            }, {});
      }
    },
    methods: {
      async accept() {
        await this.setCookie('cookie_bar_accecpted', true);
        this.display = false;
      },
      setCookie(name, value, days) {
        let expires = "";
        if (days) {
          const date = new Date();
          date.setTime(date.getTime() + (days * 24 * 60 * 60 * 1000));
          expires = "; expires=" + date.toUTCString();
        }
        document.cookie = name + "=" + (value || "") + expires + "; path=/";
      },
      getCookie(name) {
        let cookieValue = null;
        if (document.cookie && document.cookie !== '') {
          const cookies = document.cookie.split(';');
          for (let i = 0; i < cookies.length; i++) {
            const cookie = cookies[i].trim();
            // Does this cookie string begin with the name we want?
            if (cookie.substring(0, name.length + 1) === (name + '=')) {
              cookieValue = decodeURIComponent(cookie.substring(name.length + 1));
              break;
            }
          }
        }
        return cookieValue;
      }
    }
  }
</script>

<style lang="scss">
  $font-size: 12px;

  .cookie-bar {
    font-family: -apple-system, BlinkMacSystemFont,
    "Segoe UI", "Roboto", "Oxygen",
    "Ubuntu", "Cantarell", "Fira Sans",
    "Droid Sans", "Helvetica Neue", sans-serif;
    position: fixed;
    left: 0;
    right: 0;
    bottom: 0;
    height: 50px;
    text-align: left;
    background-color: #222;
    color: #ffffff;
    font-size: $font-size;
    display: flex;
    padding-left: 20px;
    padding-right: 20px;

    .cookie-bar-wrapper {
      display: flex;
      flex-direction: row;
      align-items: center;
      justify-content: space-between;
      flex: 1;
    }

    .cookie-bar-container {
      max-width: 1240px;
      width: 100%;
      margin-left: auto;
      margin-right: auto;
    }

    .cookie-bar-button {
      border-radius: 7px;
      padding: 7px 9px;
      appearance: none;
      border: none;
      font-weight: 500;
      font-size: $font-size / 1.05;
      background-color: #38C369;
      color: white;
    }
  }

  .slide-out-leave-active,
  .slide-out-enter-active {
    transition: .3s cubic-bezier(0.190, 1.000, 0.220, 1.000);
  }

  .slide-out-enter {
    transform: translate(0, 100%);
  }

  .slide-out-leave-to {
    transform: translate(0, 100%);
  }
</style>
