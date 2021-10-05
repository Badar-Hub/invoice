<template>
  <div @keypress.="" id="invoice" class="width-center bordered">
    <div class="row">
      <div class="col-xs-12 col-sm-6 q-px-sm">
        <img src="@/assets/logo.png" class="header-logo q-py-sm" />
      </div>
      <div class="col-xs-12 col-sm-6 q-my-auto q-px-sm">
        <h4 class="q-my-sm text-right">{{ data.type }}</h4>
        <q-input
          :label="labels.invoiceNo"
          class="q-py-sm"
          v-model="data.invoiceNo"
          outlined
        />
      </div>
    </div>
    <div class="row">
      <div class="col-xs-12 col-sm-6 q-my-auto q-px-sm">
        <h4 class="q-my-sm">{{ data.companyName }}</h4>
      </div>
      <div class="col-xs-12 col-sm-6 q-px-sm">
        <q-input
          :label="labels.ntnNumber"
          class="q-my-sm"
          outlined
          v-model="data.ntnNumber"
        />
        <q-input
          :label="labels.strnNumber"
          class="q-my-sm"
          outlined
          v-model="data.strnNumber"
        />
      </div>
    </div>
    <div class="row">
      <div class="col-xs-12 col-sm-3 q-pl-sm">
        <q-input
          class="q-pa-xs"
          type="textarea"
          :label="labels.billTo"
          v-model="billTo"
          outlined
        />
      </div>
      <div class="col-xs-12 col-sm-3 q-pr-sm">
        <q-input
          class="q-pa-xs"
          type="textarea"
          :label="labels.shipTo"
          v-model="shipTo"
          outlined
        />
      </div>
    </div>
    <div class="row q-my-sm header">
      <div
        :class="
          data.type === 'DELIVERY CHALLAN'
            ? 'col-xs-12 col-sm-9 q-px-sm'
            : 'col-xs-12 col-sm-6 q-px-sm'
        "
      >
        <p class="text-body1 q-my-sm">{{ labels.itemsLabel.item }}</p>
      </div>
      <div
        :class="
          data.type === 'DELIVERY CHALLAN'
            ? 'col-xs-12 col-sm-2 q-px-sm'
            : 'col-xs-12 col-sm-1 q-px-sm'
        "
      >
        <p class="text-body1 q-my-sm">{{ labels.itemsLabel.quantity }}</p>
      </div>
      <div
        v-if="!(data.type === 'DELIVERY CHALLAN')"
        class="col-xs-12 col-sm-2 q-px-sm"
      >
        <p class="text-body1 q-my-sm">{{ labels.itemsLabel.rate }}</p>
      </div>
      <div
        v-if="!(data.type === 'DELIVERY CHALLAN')"
        class="col-xs-12 col-sm-2 q-px-sm"
      >
        <p class="text-body1 q-my-sm">{{ labels.itemsLabel.amount }}</p>
      </div>
    </div>
    <div
      @mouseenter="hoverMouse = true"
      @mouseleave="hoverMouse = false"
      v-for="(item, index) in data.items"
      :key="index"
      class="row"
    >
      <div
        :class="
          data.type === 'DELIVERY CHALLAN'
            ? 'col-xs-12 col-sm-9 q-px-sm q-my-sm'
            : 'col-xs-12 col-sm-6 q-px-sm q-my-sm'
        "
      >
        <q-input
          label="Description of service or product..."
          outlined
          v-model="item.name"
        />
      </div>
      <div
        :class="
          data.type === 'DELIVERY CHALLAN'
            ? 'col-xs-12 col-sm-2 q-px-sm q-my-sm'
            : 'col-xs-12 col-sm-1 q-px-sm q-my-sm'
        "
      >
        <q-input label="Qty" outlined type="number" v-model="item.quantity" />
      </div>
      <div
        v-if="!(data.type === 'DELIVERY CHALLAN')"
        class="col-xs-12 col-sm-2 q-px-sm q-my-sm"
      >
        <q-input label="Rate" outlined type="number" v-model="item.rate" />
      </div>
      <div
        v-if="!(data.type === 'DELIVERY CHALLAN')"
        class="col-xs-12 col-sm-2 q-px-sm q-my-sm"
      >
        <q-input
          label="Amount"
          outlined
          type="number"
          :value="(item.amount = item.rate * item.quantity)"
        />
      </div>
      <div v-show="hoverMouse" class="col-xs-12 col-sm-1 q-my-auto">
        <q-icon
          class="cursor-pointer"
          @click="removeItem(index)"
          color="red"
          name="close"
          size="sm"
        />
      </div>
    </div>
    <div class="row text-left q-px-md q-my-sm">
      <q-btn
        rounded
        color="primary"
        :label="labels.addBtnLabel"
        @click="addNewItem"
      />
    </div>
    <div class="row">
      <div class="col-xs-12 col-sm-6 q-px-md">
        <q-input
          class="q-my-sm"
          type="textarea"
          :label="labels.notes"
          outlined
          v-model="data.notes"
        />
      </div>
      <div
        v-if="data.type === 'INVOICE' || data.type === 'SALE TAX INVOICE'"
        class="col-xs-12 col-sm-6 q-px-sm"
      >
        <q-input
          :label="labels.subtotal"
          outlined
          class="q-px-xl q-my-sm"
          v-model="data.subtotal"
        />
        <q-input
          :label="labels.total"
          outlined
          class="q-px-xl q-my-sm"
          v-model="data.total"
        />
        <q-input
          v-show="data.type === 'SALE TAX INVOICE'"
          :label="labels.gst"
          outlined
          class="q-px-xl q-my-sm"
          v-model="data.amountPaid"
        />
        <q-input
          :label="labels.balanceDue"
          outlined
          class="q-px-xl q-my-sm"
          v-model="data.balanceDue"
        />
      </div>
    </div>
  </div>
  <q-btn
    class="q-my-xl"
    label="Change Labels"
    @click="changeLabel = !changeLabel"
  />
  <q-btn class="q-my-xl" @click="printInvoice" label="Print Invoice" />
  <Modal
    @close="changeLabel = !changeLabel"
    title="Change Labels"
    v-model="changeLabel"
  >
    <div class="row">
      <div class="col-xs-12 col-sm-6 q-px-sm q-my-sm">
        <q-select
          label="Invoice Type"
          :options="invoiceTypes"
          v-model="data.type"
        />
      </div>
      <div class="col-xs-12 col-sm-6 q-px-sm q-my-sm">
        <q-input
          label="Invoice Number"
          :options="invoiceTypes"
          v-model="labels.invoiceNo"
        />
      </div>
      <div class="col-xs-12 col-sm-6 q-px-sm q-my-sm">
        <q-input label="NTN Number" v-model="labels.ntnNumber" />
      </div>
      <div class="col-xs-12 col-sm-6 q-px-sm q-my-sm">
        <q-input label="STRN Number" v-model="labels.strnNumber" />
      </div>
      <div class="col-xs-12 col-sm-6 q-px-sm q-my-sm">
        <q-input label="Bill To" v-model="labels.billTo" />
      </div>
      <div class="col-xs-12 col-sm-6 q-px-sm q-my-sm">
        <q-input label="Ship To" v-model="labels.shipTo" />
      </div>
      <div class="col-xs-12 col-sm-6 q-px-sm q-my-sm">
        <q-input label="Company Name" v-model="data.companyName" />
      </div>
      <div class="col-xs-12 col-sm-6 q-px-sm q-my-sm">
        <q-input label="Item" v-model="labels.itemsLabel.item" />
      </div>
      <div class="col-xs-12 col-sm-6 q-px-sm q-my-sm">
        <q-input label="Quantity" v-model="labels.itemsLabel.quantity" />
      </div>
      <div class="col-xs-12 col-sm-6 q-px-sm q-my-sm">
        <q-input label="Rate" v-model="labels.itemsLabel.rate" />
      </div>
      <div class="col-xs-12 col-sm-6 q-px-sm q-my-sm">
        <q-input label="Amount" v-model="labels.itemsLabel.amount" />
      </div>
      <div class="col-xs-12 col-sm-6 q-px-sm q-my-sm">
        <q-input label="Notes" v-model="labels.notes" />
      </div>
      <div class="col-xs-12 col-sm-6 q-px-sm q-my-sm">
        <q-input label="Subtotal" v-model="labels.subtotal" />
      </div>
      <div class="col-xs-12 col-sm-6 q-px-sm q-my-sm">
        <q-input label="Total" v-model="labels.total" />
      </div>
      <div class="col-xs-12 col-sm-6 q-px-sm q-my-sm">
        <q-input label="GST" v-model="labels.gst" />
      </div>
      <div class="col-xs-12 col-sm-6 q-px-sm q-my-sm">
        <q-input label="Balance Due" v-model="labels.balanceDue" />
      </div>
    </div>
    <div class="row">
      <div class="col-xs-12 col-sm-6 q-pa-sm">
        <q-btn
          label="Submit"
          class="full-width"
          color="primary"
          @click="changeLabel = !changeLabel"
        />
      </div>
      <div class="col-xs-12 col-sm-6 q-pa-sm">
        <q-btn
          label="Close"
          class="full-width"
          color="primary"
          @click="changeLabel = !changeLabel"
        />
      </div>
    </div>
  </Modal>
</template>

<script>
import { ref, defineComponent } from 'vue';
import print from 'print-js';
import Modal from './Modal.vue';

export default defineComponent({
  components: { Modal },
  setup() {
    const data = ref({
      type: 'INVOICE',
      invoiceNo: 0,
      companyName: 'Tech Origin',
      ntnNumber: 0,
      strnNumber: 0,
      billTo: '',
      shipTo: '',
      items: [
        {
          name: '',
          quantity: 0,
          rate: 0,
          amount: 0,
        },
      ],
      notes: '',
      subtotal: 0,
      gst: 0,
      amountPaid: 0,
      total: 0,
      balanceDue: 0,
    });
    const labels = ref({
      headingTitle: 'Invoice',
      invoiceNo: 'Invoice Number',
      companyName: 'Company Name',
      ntnNumber: 'NTN Number',
      strnNumber: 'STRN Number',
      address: 'Address',
      billTo: 'Bill To',
      shipTo: 'Shop To',
      items: {
        name: 'Description of service or product...',
        quantity: 0,
        rate: 0,
        amount: 0,
      },
      itemsLabel: {
        item: 'Item',
        quantity: 'Quantity',
        rate: 'Rate',
        amount: 'Amount',
      },
      addBtnLabel: 'Add New Line',
      notes: 'Notes',
      subtotal: 'Sub Total',
      gst: 'GST',
      amountPaid: 'Amount Paid',
      total: 'Total',
      balanceDue: 'Balance Due',
    });
    const changeLabel = ref(false);
    const invoiceTypes = ref([
      'INVOICE',
      'SALE TAX INVOICE',
      'DELIVERY CHALLAN',
    ]);
    const hoverMouse = ref(false);
    const printInvoice = () => {
      print('invoice', 'html');
    };
    const addNewItem = () => {
      data.value.items.push({
        name: '',
        quantity: 0,
        rate: 0,
        amount: 0,
      });
    };

    const removeItem = (currentIndex) => {
      data.value.items = data.value.items.filter(
        (x, index) => index !== currentIndex
      );
    };

    return {
      data,
      labels,
      removeItem,
      addNewItem,
      hoverMouse,
      changeLabel,
      printInvoice,
      invoiceTypes,
    };
  },
});
</script>

<style lang="scss">
.header-logo {
  max-width: 150px;
  max-height: 150px;
  height: auto;
  width: 100%;
}
.header {
  background-color: #343a40;
  color: white;
  border-radius: 2%;
}
.bordered {
  border: 1px solid rgb(128, 128, 128);
  border-radius: 2%;
}
</style>
