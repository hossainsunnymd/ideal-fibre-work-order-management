<script setup>
import { ref } from "vue";
import InvoiceDetails from "./InvoiceDetails.vue";
// import InvoiceAllDetails from "./InvoiceAllDetails.vue";
import { Link, usePage, useForm, router } from "@inertiajs/vue3";
import { createToaster } from "@meforma/vue-toaster";
import InvoiceDetailsList from "./InvoiceDetailsList.vue";

const toaster = createToaster({});

const show = ref(false);
const customer = ref();
const page = usePage();

const detail=ref(false);

const searchValue = ref();
const searchField = ref(["customer.name", "id"]);
const headers = [
  { text: "Invoice no", value: "id" },
  { text: "Party Name", value: "customer.name" },
  { text: "Moblie", value: "customer.mobile" },
  { text: "Total Order", value: "total_work_order" },
  { text: "Amount", value: "total" },
  { text: "Created By", value: "created_by" },
  { text: "Created Date", value: "created_at" },
  { text: "Action", value: "action" },
];

const items = ref(page.props.list);

const deleteInvoice = (id) => {
  if (confirm("Are you sure you want to delete this Invoice?")) {
    router.get(`/delete-invoice?id=${id}`);
  }
};

if (page.props.flash.status === true) {
  toaster.success(page.props.flash.message);
}

const showDetails = (id) => {
  show.value = !show.value;
  customer.value = items.value.find((item) => item.id === id);
};

const showAllDetails = () => {
    detail.value = !detail.value
}
const fromDate = new URLSearchParams(window.location.search).get("fromDate");
const toDate = new URLSearchParams(window.location.search).get("toDate");
const searchForm = ref({
  fromDate: fromDate?fromDate:"",
  toDate: toDate?toDate:"",
  customerId: "",
});

const search = () => {
  router.get(
    `/invoice-page?fromDate=${searchForm.value.fromDate}&toDate=${searchForm.value.toDate}&customerId=${searchForm.value.customerId}`
  );
};

const formatDate = (date) => {
  const d = new Date(date).toLocaleString();
  return d;
};
</script>

<template>
  <div class="p-4 bg-[#f8f8f8] ">
    <h1 class="text-2xl font-bold mb-4">Work Order List / Invoices</h1>
    <InvoiceDetails v-model:show="show" :customer="customer" />
    <InvoiceDetailsList v-model:show="detail" :items="items" />

    <div class="row mb-4">
      <div class="col-md-3 mb-3">
        <select v-model="searchForm.customerId" class="form-select">
          <option value="" disabled selected>Select Customer</option>
          <option v-for="customer in page.props.customerList" :key="customer.id" :value="customer.id">{{ customer.name }}</option>
        </select>
      </div>

      <div class="col-md-3 mb-3">
        <!-- <label class="form-label">Search From:</label> -->
        <input v-model="searchForm.fromDate" type="date" class="form-control" />
      </div>

      <div class="col-md-3 mb-3">
        <!-- <label class="form-label">To:</label> -->
        <input v-model="searchForm.toDate" type="date" class="form-control" />
      </div>

      <div class="col-md-3 mb-3">
        <button @click="search" class="btn btn-success btn-sm mr-4">Search</button>
        <Link href="/invoice-page" class="btn btn-secondary btn-sm">Clear Search</Link>
        <button class="btn btn-primary btn-sm my-2 bg-gradient-primary" @click="showAllDetails()">View all</button>
      </div>


    </div>


    <div class="p-4 bg-[#f8f8f8]">
      <EasyDataTable
        buttons-pagination
        alternating
        :headers="headers"
        :items="items"
        :search-value="searchValue"
        :search-field="searchField"
        :rows-per-page="20"
      >
        <template #item-action="{ id }">
          <button class="btn btn-outline-dark text-sm px-3 py-1 btn-sm m-1" @click="showDetails(id)">
            <i class="fa text-sm fa-eye"></i>
          </button>

          <button class="btn btn-danger btn-sm" @click="deleteInvoice(id)">Delete</button>
        </template>

        <template #item-created_at="{ created_at }">
          {{ formatDate(created_at) }}
        </template>
      </EasyDataTable>
    </div>
  </div>
</template>
