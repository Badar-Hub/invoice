<template>
  <div class="width-center">
    <div class="row">
      <div class="col-xs-12 col-sm-6 q-px-xl">
        <img src="@/assets/logo.png" class="header-logo q-py-sm" />
      </div>
      <div class="col-xs-12 col-sm-6 q-my-auto q-px-xl">
        <q-input
          label="INVOICE"
          class="q-py-sm"
          v-model="data.headingTitle"
          outlined
        />
        <q-input
          label="Invoice No:"
          class="q-py-sm"
          v-model="data.invoiceNo"
          outlined
        />
      </div>
    </div>
    <div class="row">
      <div class="col-xs-12 col-sm-6 q-my-auto q-px-xl">
        <q-input
          label="Address"
          autogrow
          class="q-my-sm"
          outlined
          v-model="data.companyName"
        />
      </div>
      <div class="col-xs-12 col-sm-6 q-px-xl">
        <q-input
          label="NTN Number"
          class="q-my-sm"
          outlined
          v-model="data.ntnNumber"
        />
        <q-input
          label="STRN Number"
          class="q-my-sm"
          outlined
          v-model="data.strnNumber"
        />
      </div>
    </div>
    <div class="row">
      <div class="col-xs-12 col-sm-3 q-pl-xl">
        <q-input
          class="q-pa-xs"
          type="textarea"
          label="Bill To"
          v-model="billTo"
          outlined
        />
      </div>
      <div class="col-xs-12 col-sm-3 q-pr-xl">
        <q-input
          class="q-pa-xs"
          type="textarea"
          label="Ship To"
          v-model="shipTo"
          outlined
        />
      </div>
    </div>
  </div>
</template>

<script>
import { ref, defineComponent } from 'vue';

export default defineComponent({
  setup() {
    const data = ref({
      headingTitle: 'Invoice',
      invoiceNo: 0,
      companyName: 'Tech Origin',
      ntnNumber: 0,
      strnNumber: 0,
      billTo: '',
      shipTo: '',
    });

    return {
      data,
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
</style>
