<template>
  <div id="wrapper" class="wrapperDiv">
    <nav class="navbar is-primary" role="navigation" aria-label="main navigation">
      <div class="navbar-menu is-flex-tablet is-flex-touch">
        <div class="navbar-start is-flex-tablet is-flex-touch">
          <a class="navbar-item is-flex-tablet is-flex-touch" @click="showProfile = true">
            Profile
          </a>
          <a class="navbar-item has-dropdown is-hoverable">
            <a class="navbar-item is-flex-tablet is-flex-touch white-text">
              Setting
            </a>
            <a class="navbar-dropdown is-boxed">
              <a class="navbar-item" @click="showInvoiceGeneratorSetting = true">
                Invoice
              </a>
            </a>
          </a>
        </div>
        <div class="navbar-end is-flex-tablet is-flex-touch">
          <a class="navbar-item is-flex-tablet is-flex-touch">
            Licenced To: Mr X
          </a>
        </div>
      </div>
    </nav>
    <section class="hero is-primary">
      <div class="hero-body">
        <div class="container has-text-centered">
          <div class="title">
            Welcome To Stock Manager
          </div>
        </div>
      </div>
      <div class="hero-foot">
        <nav class="tabs is-boxed is-fullwidth">
          <div class="container">
            <ul>
              <transition>
                <router-link to="/" tag="li" activeClass="is-active" class="increase-Font" exact><a>Stock
                  Master</a></router-link>
              </transition>
              <router-link to="/party" tag="li" activeClass="is-active" class="increase-Font" exact><a>Party Master</a>
              </router-link>
              <router-link to="/sales" tag="li" activeClass="is-active" class="increase-Font" exact><a>Sales</a>
              </router-link>
              <router-link to="/purchase" tag="li" activeClass="is-active" class="increase-Font" exact><a>Purchase
                Entry</a></router-link>
              <router-link to="/debitNote" tag="li" activeClass="is-active" class="increase-Font" exact><a>Debit Note</a>
              </router-link>
              <router-link to="/creditNote" tag="li" activeClass="is-active" class="increase-Font" exact><a>Credit Note</a>
              </router-link>
              <router-link to="/delivery" tag="li" activeClass="is-active" class="increase-Font" exact><a>Delivery
                Challan</a></router-link>
              <router-link to="/report" tag="li" activeClass="is-active" class="increase-Font" exact><a>Report</a>
              </router-link>
            </ul>
          </div>
        </nav>
      </div>
    </section>
    <app-profile :show="showProfile" @toggle="showProfile = $event"></app-profile>
    <app-invoice-setting :show="showInvoiceGeneratorSetting" @toggle="showInvoiceGeneratorSetting = $event"></app-invoice-setting>
  </div>
</template>

<script>
import appProfile from "./widgets/profile";
import appInvoiceSetting from "./widgets/invoiceSetting";

export default {
  name: "landing-page",
  components: {
    appProfile,
    appInvoiceSetting,
  },
  data() {
    return {
      showBillFormatSetting: false,
      showInvoiceGeneratorSetting: false,
      showUQCCode: false,
      showParty: false,
      showProfile: false,
      db: {},
    };
  },
  created() {
    this.$store.dispatch("initInvoiceState");
    this.$store.dispatch("initProfileState");
    this.$store.dispatch("initUQCCodeState");
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Source+Sans+Pro");

.increase-Font {
  font-size: 1.25em;
}

.white-text {
  color: white;
}
</style>
