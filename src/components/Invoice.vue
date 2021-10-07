<template>
  <div class="width-center">
    <div class="row justify-between">
      <div class="col-xs-12 col-sm-6 q-px-sm">
        <img src="@/assets/logo.png" class="header-logo q-py-sm" />
      </div>
      <div class="col-xs-12 col-sm-3 q-my-auto q-px-sm">
        <h4 class="q-my-sm text-right">{{ data.type }}</h4>
      </div>
    </div>
    <div class="row justify-between q-px-sm">
      <div class="col-xs-12 col-sm-6 q-my-auto q-px-sm">
        <h3 class="q-my-none">{{ data.companyName }}</h3>
      </div>
      <div class="col-xs-12 col-sm-3">
        <q-input
          :label="labels.invoiceNo"
          class="q-py-sm"
          v-model="data.invoiceNo"
          :borderless="generateInvoice"
        />
      </div>
    </div>
    <div
      v-show="data.type === 'SALE TAX INVOICE'"
      class="row justify-end q-my-sm"
    >
      <div class="col-xs-12 col-sm-2 q-px-sm text-right">
        <q-input
          :label="labels.ntnNumber"
          class="q-my-sm"
          :borderless="generateInvoice"
          v-model="data.ntnNumber"
        />
      </div>
      <div class="col-xs-12 col-sm-2">
        <q-input
          :label="labels.strnNumber"
          class="q-my-sm"
          :borderless="generateInvoice"
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
          v-model="data.billTo"
          borderless
        />
      </div>
      <div class="col-xs-12 col-sm-3 q-pr-sm">
        <q-input
          class="q-pa-xs"
          type="textarea"
          :label="labels.shipTo"
          v-model="data.shipTo"
          borderless
        />
      </div>
    </div>
    <div class="row q-my-sm header justify-start">
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
      :class="data.type === 'DELIVERY CHALLAN' ? 'justify-centet' : ''"
      class="row"
    >
      <div
        :class="
          data.type === 'DELIVERY CHALLAN'
            ? 'col-xs-12 col-sm-9 q-px-sm q-my-xs'
            : 'col-xs-12 col-sm-6 q-px-sm q-my-xs'
        "
      >
        <q-input
          :label="item.name ? '' : 'Description of service or product...'"
          :outlined="!generateInvoice"
          :borderless="generateInvoice"
          v-model="item.name"
        />
      </div>
      <div
        :class="
          data.type === 'DELIVERY CHALLAN'
            ? 'col-xs-12 col-sm-2 q-px-sm q-my-auto q-mx-sm'
            : 'col-xs-12 col-sm-1 q-px-sm q-my-auto q-mx-sm'
        "
      >
        <q-input
          v-if="!item.quantity"
          :borderless="generateInvoice"
          :label="item.quantity ? '' : 'Qty'"
          :outlined="!generateInvoice"
          type="number"
          v-model="item.quantity"
        />
        <h6 v-else class="text-body1 q-my-xs q-pt-md">{{ item.quantity }}</h6>
      </div>
      <div
        v-if="!(data.type === 'DELIVERY CHALLAN')"
        class="col-xs-12 col-sm-2 q-px-sm q-my-auto"
      >
        <q-input
          v-if="!generateInvoice"
          :borderless="generateInvoice"
          :label="item.rate ? '' : 'Rate'"
          :outlined="!generateInvoice"
          type="number"
          v-model="item.rate"
        />
        <h6 v-else class="text-body1 q-my-xs q-pt-md">PKR {{ item.rate }}</h6>
      </div>
      <div
        v-if="!(data.type === 'DELIVERY CHALLAN')"
        class="col-xs-12 col-sm-2 q-px-sm q-my-auto"
      >
        <h6 class="q-my-xs text-body1 q-pt-md">
          PKR
          {{
            (item.quantity * item.rate)
              .toString()
              .replace(/\B(?=(\d{3})+(?!\d))/g, ',')
          }}
        </h6>
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
        v-show="!generateInvoice"
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
          :label="labels.notes"
          :borderless="generateInvoice"
          v-model="data.notes"
        />
      </div>
      <div
        v-if="!(data.type === 'DELIVERY CHALLAN')"
        class="col-xs-12 col-sm-6 q-px-sm text-right q-px-xl"
      >
        <q-input
          v-if="!generateInvoice"
          :label="labels.subtotal"
          :outlined="!generateInvoice"
          class="q-px-xl q-my-sm"
          v-model="data.subtotal"
        />
        <h6 v-else class="q-my-sm">
          <strong> {{ labels.subtotal }}</strong
          >: PKR
          {{ data.subtotal.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',') }}
        </h6>
        <q-input
          v-if="!generateInvoice"
          v-show="data.type === 'SALE TAX INVOICE'"
          :label="labels.gst"
          :outlined="!generateInvoice"
          class="q-px-xl q-my-sm"
          v-model="data.gst"
        />
        <h6 v-show="data.type === 'SALE TAX INVOICE'" v-else class="q-my-sm">
          <strong> GST (-{{ data.gst }}%)</strong>: - PKR
          {{
            ((data.subtotal * 16) / 100)
              .toString()
              .replace(/\B(?=(\d{3})+(?!\d))/g, ',')
          }}
        </h6>
        <q-input
          v-if="!generateInvoice"
          :label="labels.total"
          :outlined="!generateInvoice"
          class="q-px-xl q-my-sm"
          v-model="data.total"
        />
        <h6 v-show="data.type === 'SALE TAX INVOICE'" v-else class="q-my-sm">
          <strong> {{ labels.total }}</strong
          >: PKR
          {{
            (data.total = Math.round(data.subtotal - (data.subtotal * 16) / 100)
              .toString()
              .replace(/\B(?=(\d{3})+(?!\d))/g, ','))
          }}
        </h6>
        <h6
          v-if="generateInvoice"
          v-show="!(data.type === 'SALE TAX INVOICE')"
          class="q-my-sm"
        >
          <strong>{{ labels.total }}</strong
          >: PKR
          {{ data.subtotal.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',') }}
        </h6>
      </div>
    </div>
  </div>
  <q-btn
    class="q-my-xl"
    label="Change Labels"
    @click="changeLabel = !changeLabel"
  />
  <q-btn class="q-my-xl" @click="generateInvoiceStore" label="Print Invoice" />
  <q-btn class="q-my-xl" @click="newInvoice" label="New Invoice" />
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
import { ref, defineComponent, onMounted } from 'vue';
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
      gst: 16,
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
      shipTo: 'Ship To',
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
      'QUOTATION',
      'INVOICE',
      'SALE TAX INVOICE',
      'DELIVERY CHALLAN',
    ]);
    const hoverMouse = ref(false);
    const printInvoice = () => {
      print('invoice', 'html');
    };
    const generateInvoice = ref(false);
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

    const generateInvoiceStore = async () => {
      try {
        generateInvoice.value = true;
        await localStorage.setItem('invoice', JSON.stringify(data.value));
        console.log(localStorage.getItem('invoice'));
      } catch (error) {
        console.log(error);
      }
    };

    const newInvoice = () => {
      localStorage.removeItem('invoice');
      data.value.type = 'INVOICE';
      data.value.invoiceNo = 0;
      data.value.gst = 0;
      data.value.amountPaid = 0;
      data.value.total = 0;
      data.value.balanceDue = 0;
      data.value.companyName = '';
      data.value.ntnNumber = 0;
      data.value.strnNumber = 0;
      data.value.billTo = '';
      data.value.shipTo = '';
      data.value.items = [];
      data.value.notes = '';
    };

    onMounted(async () => {
      try {
        const dataLocal = localStorage.getItem('invoice');
        console.log(dataLocal);
        if (dataLocal) {
          data.value = await JSON.parse(localStorage.getItem('invoice'));
        }
        console.log(data.value);
      } catch (error) {
        console.log(error);
      }
    });

    return {
      data,
      labels,
      removeItem,
      addNewItem,
      hoverMouse,
      newInvoice,
      changeLabel,
      printInvoice,
      invoiceTypes,
      generateInvoice,
      generateInvoiceStore,
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
