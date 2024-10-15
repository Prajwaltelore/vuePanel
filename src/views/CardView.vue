<template>
  <v-breadcrumbs :items="items" class="mt-2 ms-4">
    <template v-slot:title="{ item }">
      {{ item.title }}
    </template>
    <template v-slot:divider>
      <v-icon icon="mdi-chevron-right"></v-icon>
    </template>
  </v-breadcrumbs>
  <v-card class="mt-5" style="width: 95%; margin: auto">
    <v-tabs v-model="tab">
      <v-tab value="one">Saved Cards</v-tab>
      <v-tab value="two">GD Cards</v-tab>
      <v-spacer></v-spacer>
      <v-btn
        prepend-icon="mdi-plus"
        @click="dialog = true"
        class="me-2 text-white"
        style="margin: auto; background-color: #00ccdd"
        >Add Card</v-btn
      >
      <v-dialog v-model="dialog" style="width: auto">
        <v-card>
          <v-card-title> New Card </v-card-title>
          <v-card-text>
            <v-row dense>
              <v-col cols="12">
                <v-text-label>Name:</v-text-label>
                <v-text-field
                  label="i.e.James Carlon"
                  v-model="form.name"
                  type="text"
                  required
                ></v-text-field>
              </v-col>

              <v-col cols="12">
                <v-text-label>Bank Name:</v-text-label>
                <v-text-field
                  label="i.e.HDFC BANK"
                  type="text"
                  v-model="form.bankName"
                  required
                ></v-text-field>
              </v-col>

              <v-col cols="12">
                <v-text-label>Card Type:</v-text-label>
                <v-select
                  :items="['Credit', 'Debit']"
                  label="Select Card Type"
                  type="text"
                  v-model="form.cardType"
                  required
                ></v-select>
              </v-col>

              <v-col cols="12">
                <v-text-label>Card Number:</v-text-label>
                <v-text-field
                  label="i.e.7754 1542 6584 4875"
                  v-model="form.cardNumber"
                  type="number"
                  required
                ></v-text-field>
              </v-col>

              <v-col cols="12" md="6" lg="6">
                <v-text-label>Valid Till:</v-text-label>
                <v-text-field
                  v-model="form.expiryDate"
                  type="date"
                ></v-text-field>
              </v-col>

              <v-col cols="12" md="4" lg="4" class="ms-2">
                <v-text-label>CVV:</v-text-label>
                <v-text-field
                  label="_ _ _"
                  type="password"
                  v-model="form.cvv"
                  required
                ></v-text-field>
              </v-col>

              <v-col cols="12">
                <v-checkbox
                  v-model="form.isDefault"
                  label="Set as Default"
                ></v-checkbox>
                <v-checkbox
                  v-model="form.addToGPay"
                  label="Add to GPay"
                ></v-checkbox>
              </v-col>
            </v-row>
          </v-card-text>

          <v-divider></v-divider>

          <v-card-actions>
            <v-spacer></v-spacer>

            <v-btn text="Close" variant="plain" @click="dialog = false"></v-btn>

            <v-btn
              color="primary"
              text="Save"
              variant="tonal"
              @click="saveCard()"
            ></v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-tabs>

    <v-card-text>
      <v-tabs-window v-model="tab">
        <v-tabs-window-item value="one">
          <v-row>
            <v-col cols="12" md="4" lg="4" sm="12">
              <div
                class="d-flex text-secondary"
                style="
                  margin: auto;
                  height: 60px;
                  align-items: center;
                  background-color: #f5f5f7;
                "
              >
                <v-icon>mdi-apps</v-icon>
                <p class="ms-1">Card Details</p>
                <v-spacer></v-spacer>
                <v-btn
                  icon
                  style="width: auto; height: auto; background-color: #bcf2f6"
                >
                  <v-icon>mdi-chevron-down</v-icon>
                </v-btn>
              </div>
              <v-card class="border border-dark mt-2">
                <div
                  class="d-flex text-secondary"
                  style="
                    margin: auto;
                    height: 60px;
                    align-items: center;
                    background-color: #f5f5f7;
                    border: none;
                  "
                >
                  <v-icon>mdi-tune-variant</v-icon>
                  <p class="ms-1">Today's Transactions</p>
                  <v-spacer></v-spacer>
                  <v-btn
                    icon
                    style="width: auto; height: auto; background-color: #bcf2f6"
                  >
                    <v-icon>mdi-chevron-down</v-icon>
                  </v-btn>
                </div>
                <List></List>
              </v-card>
            </v-col>
            <v-col cols="12" md="8" lg="8" sm="12">
              <h2 class="text-secondary mt-5">Credit Cards</h2>
              <hr
                style="width: 13%; margin-left: 0; border: 2px solid #bcf2f6"
              />
              <v-row>
                <v-col cols="12" md="6" lg="6" sm="12">
                  <div class="mt-5 d-flex">
                    <v-spacer></v-spacer>
                    <v-btn
                      class="text-end"
                      prepend-icon="mdi-eye"
                      style="background-color: #bcf2f6; height: 60%"
                      @click="showFullCardNumber = !showFullCardNumber"
                    >
                      <template v-slot:prepend>
                        <v-icon></v-icon>
                      </template>
                      Show Card Number
                    </v-btn>
                  </div>
                  <v-carousel
                    :continuous="false"
                    :show-arrows="false"
                    delimiter-icon="mdi-square"
                    height="250"
                    hide-delimiter-background
                    v-model="activeCardCreditIndex"
                  >
                    <v-carousel-item v-for="(card, i) in creditCards" :key="i">
                      <v-card
                        class="mt-2 text-white"
                        :class="cardClass(card)"
                        :style="{
                          pointerEvents: card.disabled ? 'none' : 'auto',
                        }"
                        style="height: 80%; background-color: #1e3e62"
                      >
                        <div class="icon-container">
                          <v-icon v-if="card.isLocked" class="icon-top-left"
                            >mdi-lock</v-icon
                          >
                          <v-icon v-if="card.isArchived" class="icon-top-left"
                            >mdi-archive</v-icon
                          >
                          <v-icon v-if="card.isDefault" class="icon-top-left"
                            >mdi-check</v-icon
                          >
                          <!-- <v-icon v-if="card.addToGPay" class="icon-top-left"
                            >G Pay</v-icon
                          > -->
                          <p v-if="card.addToGPay" class="icon-top-left">
                            G Pay
                          </p>
                        </div>
                        <v-card-title class="text-end">{{
                          card.bankName
                        }}</v-card-title>
                        <v-card-text class="mt-5">
                          <h2>{{ card.name }}</h2>
                          <p style="padding: 5px">
                            {{
                              showFullCardNumber
                                ? card.cardNumber
                                : "**** **** **** " + card.cardNumber.slice(-4)
                            }}
                          </p>
                          <div class="d-flex mt-2" style="align-items: center">
                            <p class="me-5">
                              Valid Till:
                              {{ formatExpiryDate(card.expiryDate) }}
                            </p>
                            <p class="ms-5">
                              CVV: {{ cvvDetails ? card.cvv : "***" }}
                            </p>
                            <v-img
                              class="ms-2"
                              src="Asset-2-5.png"
                              :width="25"
                              :height="35"
                            ></v-img>
                          </div>
                        </v-card-text>
                      </v-card>
                    </v-carousel-item>
                  </v-carousel>
                </v-col>
                <v-col cols="12" md="6" lg="6" sm="12">
                  <div
                    class="w-50 d-flex"
                    style="
                      height: 200px;
                      background-color: #bcf2f6;
                      border-radius: 5px;
                      align-items: center;
                      margin-top: 46px;
                      margin-left: auto;
                      margin-right: auto;
                    "
                  >
                    <v-row style="width: 100%; height: 100%">
                      <v-col
                        cols="6"
                        class="d-flex"
                        style="
                          flex-direction: column;
                          align-items: center;
                          justify-content: center;
                        "
                      >
                        <v-btn
                          icon
                          @click="
                            toggleLockCard(activeCardCreditIndex, 'Credit')
                          "
                          style="background-color: #7cf5ff"
                        >
                          <v-icon>mdi-lock</v-icon>
                        </v-btn>
                        Lock Card
                      </v-col>
                      <v-col
                        cols="6"
                        class="d-flex"
                        style="
                          flex-direction: column;
                          align-items: center;
                          justify-content: center;
                        "
                      >
                        <v-btn
                          icon
                          @click="
                            toggleArchiveCard(activeCardCreditIndex, 'Credit')
                          "
                          style="background-color: #7cf5ff"
                        >
                          <v-icon>mdi-archive</v-icon>
                        </v-btn>
                        Archive
                      </v-col>
                      <v-col
                        cols="6"
                        class="d-flex"
                        style="
                          flex-direction: column;
                          align-items: center;
                          justify-content: center;
                        "
                      >
                        <v-btn
                          icon
                          @click="setAsDefault(activeCardCreditIndex, 'Credit')"
                          style="background-color: #7cf5ff"
                        >
                          <v-icon>mdi-check</v-icon>
                        </v-btn>
                        Set As <br />
                        Default
                      </v-col>
                      <v-col
                        cols="6"
                        class="d-flex"
                        style="
                          flex-direction: column;
                          align-items: center;
                          justify-content: center;
                        "
                      >
                        <v-btn
                          icon
                          @click="addToGPay(activeCardCreditIndex, 'Credit')"
                          style="background-color: #7cf5ff"
                        >
                          GPay
                        </v-btn>
                        Add to <br />
                        GPay
                      </v-col>
                    </v-row>
                  </div>
                </v-col>
              </v-row>
              <h2 class="text-secondary mt-5">Debit Cards</h2>
              <hr
                style="width: 13%; margin-left: 0; border: 2px solid #bcf2f6"
              />
              <v-row>
                <v-col cols="12" md="6" lg="6" sm="12">
                  <div class="mt-5 d-flex">
                    <v-spacer></v-spacer>
                    <v-btn
                      class="text-end"
                      prepend-icon="mdi-eye"
                      style="background-color: #bcf2f6; height: 60%"
                      @click="showFullCardNumber = !showFullCardNumber"
                    >
                      <template v-slot:prepend>
                        <v-icon></v-icon>
                      </template>
                      Show Card Number
                    </v-btn>
                  </div>
                  <v-carousel
                    :continuous="false"
                    :show-arrows="false"
                    delimiter-icon="mdi-square"
                    height="250"
                    hide-delimiter-background
                    v-model="activeCardDebitIndex"
                  >
                    <v-carousel-item v-for="(card, i) in debitCards" :key="i">
                      <v-card
                        class="mt-2 text-white"
                        :class="cardClass(card)"
                        :style="{
                          pointerEvents: card.disabled ? 'none' : 'auto',
                        }"
                        style="height: 80%; background-color: #1e3e62"
                      >
                        <div class="icon-container">
                          <v-icon v-if="card.isLocked" class="icon-top-left"
                            >mdi-lock</v-icon
                          >
                          <v-icon v-if="card.isArchived" class="icon-top-left"
                            >mdi-archive</v-icon
                          >
                          <v-icon v-if="card.isDefault" class="icon-top-left"
                            >mdi-check</v-icon
                          >
                          <!-- <v-icon v-if="card.addToGPay" class="icon-top-left"
                            >G Pay</v-icon
                          > -->
                          <p v-if="card.addToGPay" class="icon-top-left">
                            G Pay
                          </p>
                        </div>
                        <v-card-title class="text-end">{{
                          card.bankName
                        }}</v-card-title>
                        <v-card-text class="mt-5">
                          <h2>{{ card.name }}</h2>
                          <p style="padding: 5px">
                            {{
                              showFullCardNumber
                                ? card.cardNumber
                                : "**** **** **** " + card.cardNumber.slice(-4)
                            }}
                          </p>
                          <div class="d-flex mt-2" style="align-items: center">
                            <p class="me-5">
                              Valid Till:
                              {{ formatExpiryDate(card.expiryDate) }}
                            </p>
                            <p class="ms-5">
                              CVV: {{ cvvDetails ? card.cvv : "***" }}
                            </p>
                            <v-img
                              class="ms-2"
                              src="Asset-2-5.png"
                              :width="25"
                              :height="35"
                            ></v-img>
                          </div>
                        </v-card-text>
                      </v-card>
                    </v-carousel-item>
                  </v-carousel>
                </v-col>
                <v-col cols="12" md="6" lg="6" sm="12">
                  <div
                    class="w-50 d-flex"
                    style="
                      height: 200px;
                      background-color: #bcf2f6;
                      border-radius: 5px;
                      align-items: center;
                      margin-top: 46px;
                      margin-left: auto;
                      margin-right: auto;
                    "
                  >
                    <v-row style="width: 100%; height: 100%">
                      <v-col
                        cols="6"
                        class="d-flex"
                        style="
                          flex-direction: column;
                          align-items: center;
                          justify-content: center;
                        "
                      >
                        <v-btn
                          icon
                          @click="toggleLockCard(activeCardDebitIndex, 'Debit')"
                          style="background-color: #7cf5ff"
                        >
                          <v-icon>mdi-lock</v-icon>
                        </v-btn>
                        Lock Card
                      </v-col>
                      <v-col
                        cols="6"
                        class="d-flex"
                        style="
                          flex-direction: column;
                          align-items: center;
                          justify-content: center;
                        "
                      >
                        <v-btn
                          icon
                          @click="
                            toggleArchiveCard(activeCardDebitIndex, 'Debit')
                          "
                          style="background-color: #7cf5ff"
                        >
                          <v-icon>mdi-archive</v-icon>
                        </v-btn>
                        Archive
                      </v-col>
                      <v-col
                        cols="6"
                        class="d-flex"
                        style="
                          flex-direction: column;
                          align-items: center;
                          justify-content: center;
                        "
                      >
                        <v-btn
                          icon
                          @click="setAsDefault(activeCardDebitIndex, 'Debit')"
                          style="background-color: #7cf5ff"
                        >
                          <v-icon>mdi-check</v-icon>
                        </v-btn>
                        Set As <br />
                        Default
                      </v-col>
                      <v-col
                        cols="6"
                        class="d-flex"
                        style="
                          flex-direction: column;
                          align-items: center;
                          justify-content: center;
                        "
                      >
                        <v-btn
                          icon
                          @click="addToGPay(activeCardDebitIndex, 'Debit')"
                          style="background-color: #7cf5ff"
                        >
                          GPay
                        </v-btn>
                        Add to <br />
                        GPay
                      </v-col>
                    </v-row>
                  </div>
                </v-col>
              </v-row>
            </v-col>
          </v-row>
        </v-tabs-window-item>

        <v-tabs-window-item value="two"> Two </v-tabs-window-item>
      </v-tabs-window>
    </v-card-text>
  </v-card>
  <Footer />
</template>
<script lang="ts">
import Footer from "@/components/Footer.vue";
import List from "@/components/List.vue";
import { defineComponent } from "vue";
interface Card {
  name: string;
  bankName: string;
  cardType: string;
  cardNumber: number;
  expiryDate: Date;
  cvv: string;
  isLocked: boolean;
  isArchived: boolean;
  isDefault: boolean;
  addToGPay: boolean;
  disabled: boolean;
}

export default defineComponent({
  components: {
    List,
    Footer,
  },

  data() {
    return {
      showFullCardNumber: false,
      cvvDetails: false,
      activeCardDebitIndex: 0,
      activeCardCreditIndex: 0,
      items: [
        {
          title: "Home",
          disabled: false,
          href: "/",
        },
        {
          title: "Cards",
          disabled: false,
          href: "/card",
        },
        {
          title: "Transactions",
          disabled: false,
          href: "/transactions",
        },
        {
          title: "Settings",
          disabled: false,
          href: "/settings",
        },
      ],
      tab: null,
      dialog: false,
      form: {
        name: "",
        bankName: "",
        cardType: "",
        cardNumber: null,
        expiryDate: null,
        cvv: "",
        isLocked: false,
        isArchived: false,
        isDefault: false,
        addToGPay: false,
        disabled: false,
      },
      debitCards: [] as Card[],
      creditCards: [] as Card[],
    };
  },
  created() {
    this.fetchUsers();
  },
  computed: {
    creditCardsList(): Card[] {
      return this.creditCards;
    },
    debitCardsList(): Card[] {
      return this.debitCards;
    },
  },
  watch: {
    // Watcher for credit card index change
    activeCardCreditIndex(newIndex) {
      console.log("Active Credit Card Index:", newIndex);
    },

    // Watcher for debit card index change (if needed)
    activeCardDebitIndex(newIndex) {
      console.log("Active Debit Card Index:", newIndex);
    },
  },
  methods: {
    saveCard() {
      // If the card is Credit, save it to creditCards
      if (this.form.cardType === "Credit") {
        let storedCreditCards = JSON.parse(
          localStorage.getItem("creditCards") || "[]"
        );
        storedCreditCards.push(this.form);
        localStorage.setItem("creditCards", JSON.stringify(storedCreditCards));
      }

      // If the card is Debit, save it to debitCards
      else if (this.form.cardType === "Debit") {
        let storedDebitCards = JSON.parse(
          localStorage.getItem("debitCards") || "[]"
        );
        storedDebitCards.push(this.form);
        localStorage.setItem("debitCards", JSON.stringify(storedDebitCards));
      }

      this.dialog = false;
      this.fetchUsers();
    },
    fetchUsers() {
      this.creditCards = JSON.parse(
        localStorage.getItem("creditCards") || "[]"
      );
      this.debitCards = JSON.parse(localStorage.getItem("debitCards") || "[]");
      console.log(this.debitCards);
      console.log(this.creditCards);
    },
    toggleLockCard(index: number, cardType: string) {
      if (cardType === "Credit") {
        this.creditCards[index].isLocked = !this.creditCards[index].isLocked;
        localStorage.setItem("creditCards", JSON.stringify(this.creditCards));
      } else if (cardType === "Debit") {
        this.debitCards[index].isLocked = !this.debitCards[index].isLocked;
        localStorage.setItem("debitCards", JSON.stringify(this.debitCards));
      }
      this.fetchUsers();
    },
    toggleArchiveCard(index: number, cardType: string) {
      if (cardType === "Credit") {
        this.creditCards[index].isArchived =
          !this.creditCards[index].isArchived;
        localStorage.setItem("creditCards", JSON.stringify(this.creditCards));
      } else if (cardType === "Debit") {
        this.debitCards[index].isArchived = !this.debitCards[index].isArchived;
        localStorage.setItem("debitCards", JSON.stringify(this.debitCards));
      }
      this.fetchUsers();
    },
    setAsDefault(index: number, cardType: string) {
      if (cardType === "Credit") {
        this.creditCards[index].isDefault = !this.creditCards[index].isDefault;
        localStorage.setItem("creditCards", JSON.stringify(this.creditCards));
      } else if (cardType === "Debit") {
        this.debitCards[index].isDefault = !this.debitCards[index].isDefault;
        localStorage.setItem("debitCards", JSON.stringify(this.debitCards));
      }
      this.fetchUsers();
    },
    addToGPay(index: number, cardType: string) {
      if (cardType === "Credit") {
        this.creditCards[index].addToGPay = !this.creditCards[index].addToGPay;
        localStorage.setItem("creditCards", JSON.stringify(this.creditCards));
      } else if (cardType === "Debit") {
        this.debitCards[index].addToGPay = !this.debitCards[index].addToGPay;
        localStorage.setItem("debitCards", JSON.stringify(this.debitCards));
      }
      this.fetchUsers();
    },
    formatExpiryDate(expiryDate: string): string {
      const date = new Date(expiryDate);

      // Get the month and year
      const month = (date.getMonth() + 1).toString().padStart(2, "0"); // Ensure two digits for month
      const year = date.getFullYear().toString().slice(-2);

      // Return in MM/YYYY format
      return `${month}/${year}`;
    },
    cardClass(card: Card) {
      return {
        "disabled-card": card.isLocked || card.isArchived,
        default: card.isDefault,
        gPay: card.addToGPay,
      };
    },
  },
});
</script>
<style>
.disabled-card {
  opacity: 0.5;
  pointer-events: none;
}
.default {
  background-color: #77cdff !important;
}
.gPay {
  background-color: #0d92f4 !important;
}
.icon-container {
  position: absolute;
  top: 10px;
  left: 10px;
  z-index: 10;
}

.icon-top-left {
  color: white;
  font-size: 24px;
}
</style>
