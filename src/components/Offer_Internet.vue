<template>
<div>
  <div>
    <div class="desc">	
      <div class="text">
        <h2>Internet</h2>
        <p>
          Bez względu na to czy jesteś klientem czy firmą, Kowalskim czy „Kowalskim sp. z o.o.”, dla nas <b>jesteś zawsze najważniejszy</b>, więc dopasujemy ofertę do Twoich wymagań.
        </p>
      </div>
      <div class="image">
        <img src="../assets/offer/internet.jpg" />
      </div>
    </div>
    <ul class="info-items">
      <li class="info-item">
        <!-- <img src="../assets/offer/canals.png" /> -->
        <h3>Oferta dla każdego</h3>
        <p>
          Oferujemy łącza światłowodowe ftth, Ethernet i bezprzewodowe, naszą ofertę kierujemy do każdego odbiorcy, dajemy możliwość korzystania z niezawodnego, szybkiego i pewnego dostępu do Internetu. 
        </p>
      </li>
      <li class="info-item">
        <!-- <img src="../assets/offer/multi.png" /> -->
        <h3>Najwyższa wydajność i niezadowność</h3>
        <p>
          Posiadamy dostęp do łącz operatorskich w oparciu o własne światłowodowe sieci, zoptymalizowane pod kątem uzyskania jak najwyższej wydajności i niezawodności.
        </p>
      </li>
      <li class="info-item">
        <!-- <img src="../assets/offer/fullhd.png" /> -->
        <h3>Dostosowany dla Ciebie</h3>
        <p>
          Nieważne czy potrzebujesz łącza do domu czy dla firmy, postaramy się zaoferować usługę która w najlepszy możliwy sposób zaspokoi Twoje wymagania.
        </p>
      </li>
      <li class="info-item">
        <!-- <img src="../assets/offer/decoder.png" /> -->
        <h3>Najszybszy internet światłowodowy</h3>
        <p>
          Nasze łącza <b>światłowodowe</b>, są wielokrotnie szybsze od łączy telefonicznych oraz średnio 10-krotnie szybsze od internetu LTE. Co ważne, możliwości światłowodu ciągle rosną. W świetle takich faktów możesz wybrać tylko jedno – Technologię ŚWIATŁOWODOWĄ.
        </p>
      </li>
    </ul>
  </div>
  <div class="container-offer-inner">
    <div class="nav">
      <h2>Czas trwania oferty:</h2>
      <ul>
        <li v-for="(item, index) in items" :key="index">
          <button :class="{'active': actualIndex == index}" @click="agreementLengthItemClick(index)">
            <div class="dot"/><p>{{item.agreement_length}}</p>
          </button>
        </li>
      </ul>
    </div>
    <transition name="offer-fade" mode="out-in">
      <ul v-bind:key="actualIndex" class="items" v-if="items">
        <ListItem v-for="(item, index) in items[actualIndex].items" :item="item" :key="index"/>
      </ul>
    </transition>
  </div>
  <div class="files" v-if="links">
    <h2>Do pobrania:</h2>
    <ul>
      <li v-for="(link, index) in links" :key="index">
        <a target="_blank" :href="link.href">
          <FontAwesomeIcon :icon="iconPdf" class="icon" /><p>{{link.title}}</p>
        </a>
      </li>
    </ul>
  </div>
</div>
</template>

<script>
import ListItem from "./Offer_List_Item.vue";
import axios from "axios";
import FontAwesomeIcon from "@fortawesome/vue-fontawesome";
import { faFilePdf } from "@fortawesome/fontawesome-free-regular";

export default {
  name: "Offer_Internet",
  data() {
    return {
      actualIndex: 0,
      items: null,
      links: null
    };
  },
  created: function() {
    this.fetchInternetOffer();
  },
  methods: {
    agreementLengthItemClick(index) {
      this.actualIndex = index;
    },
    fetchInternetOffer() {
      var self = this;
      axios
        .get("/static/api/offer_internet.json")
        .then(function(resp) {
          self.items = resp.data;
        })
        .catch(function(error) {
          console.log(error);
        });
      axios
        .get("/static/api/offer_internet_links.json")
        .then(function(resp) {
          self.links = resp.data;
        })
        .catch(function(error) {
          console.log(error);
        });
    }
  },
  components: {
    ListItem: ListItem,
    FontAwesomeIcon
  },
  computed: {
    iconPdf() {
      return faFilePdf;
    }
  }
};
</script>

<style lang="scss" scoped>
@import "../styles/_colors.scss";

.files h2,
.nav h2 {
  font-size: 1.125em;
  color: $colorPrimary;
  font-family: inherit;
}

.container-offer-inner {
  margin-top: 32px;
  display: flex;
  width: 100%;

  .offer-fade-enter-active,
  .offer-fade-leave-active {
    transition: all 0.2s ease;
  }

  .offer-fade-enter,
  .offer-fade-leave-to {
    opacity: 0;
  }

  .offer-fade-enter {
    transform: translateX(8px);
  }

  .offer-fade-leave-to {
    transform: translateX(-8px);
  }

  .nav {
    flex: 1;
    min-width: 210px;
    padding-bottom: 16px;

    ul {
      margin-top: 16px;
    }

    button {
      background: transparent;
      border: none;
      font-size: 0.75em;
      font-weight: 600;
      outline: none;
      margin: 8px 0;
      cursor: pointer;
      font-family: inherit;
      transition: color 0.3s ease;
      padding: 0;

      &.active,
      &:hover {
        color: $colorPrimary;
      }

      &.active .dot {
        background-color: $colorPrimary;
      }

      .dot {
        width: 8px;
        height: 8px;
        border-radius: 4px;
        display: inline-block;
        transition: background-color 0.3s ease;
      }

      p {
        margin-left: 8px;
        display: inline;
      }
    }
  }

  .items {
    flex: 3;
    display: flex;
    flex-flow: row wrap;
    justify-content: flex-start;
    margin: 0 -8px;

    .item {
      margin: {
        left: 8px;
        right: 8px;
      }
    }
  }

  @media all and (max-width: 499px) {
    flex-direction: column;

    .item {
      width: 100%;
    }

    .nav {
      text-align: center;
    }
  }

  @media all and (min-width: 500px) {
    .item {
      width: calc(100% - 16px) !important;
    }
  }

  @media all and (min-width: 700px) {
    .item {
      width: calc(49.9999% - 16px) !important;
    }
  }

  @media all and (min-width: 992px) {
    .item {
      width: calc(33.3333% - 16px) !important;
    }
  }
}
</style>
