<template>
  <md-toolbar id="toolbar" md-elevation="0" class="md-transparent md-absolute" :class="extraNavClasses" :color-on-scroll="colorOnScroll">
    <div class="md-toolbar-row md-collapse-lateral">
      <div class="md-toolbar-section-start">
        <!-- <h3 class="md-title">CIRII</h3> -->
        <a class="navbar-brand" href="#">
          <img src="../../public/logos/CIRII-LogoV4-LightBlue.png" alt="" width="125" height="100" />
        </a>
      </div>
      <div class="md-toolbar-section-end">
        <md-button class="md-just-icon md-simple md-toolbar-toggle" :class="{ toggled: toggledClass }" @click="toggleNavbarMobile()">
          <span class="icon-bar"></span>
          <span class="icon-bar"></span>
          <span class="icon-bar"></span>
        </md-button>

        <div class="md-collapse">
          <div class="md-collapse-wrapper">
            <mobile-menu nav-mobile-section-start="false">
              <!-- Here you can add your items from the section-start of your toolbar -->
            </mobile-menu>
            <md-list>
              <li class="md-list-item" v-if="!showDownload">
                <a href="javascript:void(0)" class="md-list-item-router md-list-item-container md-button-clean dropdown">
                  <div class="md-list-item-content">
                    <drop-down direction="down">
                      <md-button slot="title" class="md-button md-button-link md-white md-simple dropdown-toggle" data-toggle="dropdown">
                        <i class="material-icons">apps</i>
                        <p>Menu</p>
                      </md-button>
                      <ul class="dropdown-menu dropdown-with-icons">
                        <li>
                          <a href="#/">
                            <i class="material-icons">house</i>
                            <p>Home</p>
                          </a>
                        </li>
                        <li>
                          <a href="#/about">
                            <i class="material-icons">layers</i>
                            <p>About Us</p>
                          </a>
                        </li>
                        <li>
                          <a href="#">
                            <i class="material-icons">content_paste</i>
                            <p>Features</p>
                          </a>
                        </li>
                      </ul>
                    </drop-down>
                  </div>
                </a>
              </li>

              <md-list-item href="https://demos.creative-tim.com/vue-material-kit/documentation/" target="_blank" v-if="showDownload">
                <i class="material-icons">content_paste</i>
                <p>Documentation</p>
              </md-list-item>

              <md-list-item href="javascript:void(0)" @click="scrollToElement()" v-if="showDownload">
                <i class="material-icons">cloud_download</i>
                <p>Download</p>
              </md-list-item>

              <li class="md-list-item" v-else>
                <a href="javascript:void(0)" class="md-list-item-router md-list-item-container md-button-clean dropdown">
                  <div class="md-list-item-content">
                    <drop-down direction="down">
                      <md-button slot="title" class="md-button md-button-link md-white md-simple dropdown-toggle" data-toggle="dropdown">
                        <i class="material-icons">view_carousel</i>
                        <p>Resources</p>
                      </md-button>
                      <ul class="dropdown-menu dropdown-with-icons">
                        <li>
                          <a href="#/datas">
                            <i class="material-icons">storage</i>
                            <p>Datas</p>
                          </a>
                        </li>
                        <li>
                          <a href="#/">
                            <i class="material-icons">view_day</i>
                            <p>Patients</p>
                          </a>
                        </li>
                        <li>
                          <a href="#/profile">
                            <i class="material-icons"> fingerprint</i>
                            <p>Doctors/Caregivers</p>
                          </a>
                        </li>
                        <li>
                          <a href="#/login">
                            <i class="material-icons">account_circle</i>
                            <p>Login</p>
                          </a>
                        </li>
                      </ul>
                    </drop-down>
                  </div>
                </a>
              </li>

              <md-list-item href="https://twitter.com/CreativeTim" target="_blank">
                <i class="fab fa-twitter"></i>
                <p class="hidden-lg">Twitter</p>
              </md-list-item>
              <md-list-item href="https://www.facebook.com/CreativeTim" target="_blank">
                <i class="fab fa-facebook-square"></i>
                <p class="hidden-lg">Facebook</p>
              </md-list-item>
              <md-list-item href="https://www.instagram.com/CreativeTimOfficial" target="_blank">
                <i class="fab fa-instagram"></i>
                <p class="hidden-lg">Instagram</p>
                <!-- <md-tooltip md-direction="bottom">Follow us on Instagram</md-tooltip> -->
              </md-list-item>
              <md-list-item>
                <p class="hidden-lg"></p>
                <!-- <md-switch @click="toggleTheme" v-model="darkModeOn" aria-label="Toggle themes">
                  <span v-if="this.theme == 'darkMode'"> Light</span>
                  <span v-else> Dark</span>
                </md-switch> -->
                <md-button @click="toggleTheme" class="md-forget-me-not md-sm" aria-label="Toggle themes">
                  <span v-if="this.theme == 'darkMode'">Light</span>
                  <span v-else>Dark</span>
                </md-button>
                <!-- <button @click="toggleTheme">
                  <md-switch v-model="theme" class="md-forget-me-not md-sm" aria-label="Toggle themes">
                    <span v-if="this.theme == 'darkMode'">Light</span>
                    <span v-else>Dark</span>
                  </md-switch>
                </button> -->
              </md-list-item>
            </md-list>
          </div>
        </div>
      </div>
    </div>
  </md-toolbar>
</template>

<script>
  let resizeTimeout;
  function resizeThrottler(actualResizeHandler) {
    // ignore resize events as long as an actualResizeHandler execution is in the queue
    if (!resizeTimeout) {
      resizeTimeout = setTimeout(() => {
        resizeTimeout = null;
        actualResizeHandler();

        // The actualResizeHandler will execute at a rate of 15fps
      }, 66);
    }
  }

  import MobileMenu from "@/layout/MobileMenu";
  export default {
    components: {
      MobileMenu,
    },
    props: {
      type: {
        type: String,
        default: "white",
        validator(value) {
          return ["white", "black", "default", "primary", "danger", "success", "warning", "info"].includes(value);
        },
      },
      colorOnScroll: {
        type: Number,
        default: 0,
      },
    },
    data() {
      return {
        darkModeOn: false,
        theme: "", //When this property is empty, the theme is set to the default theme i.e. light mode.
        extraNavClasses: "",
        toggledClass: false,
      };
    },
    computed: {
      showDownload() {
        const excludedRoutes = ["index", "login", "about", "datas", "profile"];
        return excludedRoutes.every((r) => r !== this.$route.name);
      },
    },
    methods: {
      toggleTheme() {
        if (this.theme != "darkMode") {
          this.extraNavClasses = "md-black";
        } else {
          this.extraNavClasses = "md-white";
        }
        this.theme = this.theme == "darkMode" ? "" : "darkMode"; //toggles theme value
        document.documentElement.setAttribute("data-theme", this.theme); // sets the data-theme attribute
        localStorage.setItem("theme", this.theme); // stores theme value on local storage
      },
      bodyClick() {
        let bodyClick = document.getElementById("bodyClick");

        if (bodyClick === null) {
          let body = document.querySelector("body");
          let elem = document.createElement("div");
          elem.setAttribute("id", "bodyClick");
          body.appendChild(elem);

          let bodyClick = document.getElementById("bodyClick");
          bodyClick.addEventListener("click", this.toggleNavbarMobile);
        } else {
          bodyClick.remove();
        }
      },
      toggleNavbarMobile() {
        this.NavbarStore.showNavbar = !this.NavbarStore.showNavbar;
        this.toggledClass = !this.toggledClass;
        this.bodyClick();
      },
      handleScroll() {
        let localTheme = localStorage.getItem("theme"); //gets stored theme value if any
        console.log("thenme ", localTheme);
        if (localTheme == "darkMode") {
          this.extraNavClasses = "md-black";
          navbarColor.classList.remove("md-transparent");
        } else {
          this.extraNavClasses = "md-white";
        }
        let scrollValue = document.body.scrollTop || document.documentElement.scrollTop;
        let navbarColor = document.getElementById("toolbar");
        this.currentScrollValue = scrollValue;
        if (this.colorOnScroll > 0 && scrollValue > this.colorOnScroll) {
          if (this.theme == "darkMode") {
            this.extraNavClasses = `md-black`;
          } else {
            this.extraNavClasses = `md-${this.type}`;
          }
          navbarColor.classList.remove("md-transparent");
        } else {
          if (this.extraNavClasses) {
            this.extraNavClasses = "";
            navbarColor.classList.add("md-transparent");
          }
        }
      },
      scrollListener() {
        resizeThrottler(this.handleScroll);
      },
      scrollToElement() {
        let element_id = document.getElementById("downloadSection");
        if (element_id) {
          element_id.scrollIntoView({ block: "end", behavior: "smooth" });
        }
      },
    },
    mounted() {
      document.addEventListener("scroll", this.scrollListener);
      let localTheme = localStorage.getItem("theme"); //gets stored theme value if any
      document.documentElement.setAttribute("data-theme", localTheme); // updates the data-theme attribute
      console.log("thenme ", localTheme);
    },
    beforeDestroy() {
      document.removeEventListener("scroll", this.scrollListener);
    },
  };
</script>
