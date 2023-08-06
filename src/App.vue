<script setup>
import {reactive} from "vue";
const data = reactive({
  invoiceNo: '',
  orderDate: '',
  customerInfo: {
    name: '',
    email: '',
    phone: ''
  },
  billingInfo: '',
  shippingInfo: '',
  items:[{
    sl: '',
    description: '',
    qty: '',
    rate: '',
    amount: ''
  },
  ],
  notes: '',
  terms: '',
  subtotal: '',
  tax: '',
  total: '',
  payment: '',
  balanceDue: '',
})

function addMoreItem(){
  data.items.push({
    sl: '',
    description: '',
    qty: '',
    rate: '',
    amount: ''
  })
}
function deleteItem(parms){
  data.items.forEach(function(item,index){
    if(parms == index){
      data.items.splice(parms, parms)
    }
  })
}

function getSubTotal(){
  let subtotal = 0
  data.items.forEach((item)=>{
    subtotal += item.amount
  })
  data.subtotal = subtotal
  return subtotal
}
function getTotal(){
  const tax = data.subtotal * data.tax / 100
  const total = data.subtotal + tax
  data.total = total
  return total
}
function getBalanceDue(){
  const balanceDue = data.total - data.payment
  data.balanceDue = balanceDue
  return balanceDue
}
function getPrint(){
  window.print()
}
</script>

<template>
  <section class="container mx-auto bg-white border border-gray-400 min-h-screen p-12">
    <!--  Invoice Heading Start  -->
    <div class="flex justify-between border-b-2 pb-6">
      <div class="border-r-2 w-1/2 h-auto">
        <h2 class="text-xl font-bold mb-4">Invoice</h2>
        <div class="space-y-1">
          <div class="flex items-center gap-2">
            <label class="text-sm font-medium" for="invoiceNumber">Invoice No : </label>
            <input v-model="data.invoiceNo" type="text" id="invoiceNumber" class="border focus:outline-none px-2 inputBorder">
          </div>
          <div class="flex items-center gap-2">
            <label class="text-sm font-medium" for="orderDate">Order Date : </label>
            <input v-model="data.orderDate" type="text" id="orderDate" class="border focus:outline-none px-2 inputBorder">
          </div>
        </div>
      </div>
      <div class="w-1/2 h-auto">
        <div class="flex flex-col items-end">
          <img src="./assets/images/logo.png" alt="logo" >
          <h1 class="text-xl font-bold">Grostore Online Store</h1>
          <p class="text-md text-gray-600">Washington, New York, USA - 254230</p>
          <p class="text-md text-gray-600"><span class="font-semibold">Phone : </span> +88225467820</p>
        </div>
      </div>
    </div>
    <!--  Invoice Heading End  -->
    <!--  Invoice Customer Info Start  -->
    <div class="flex justify-between items-baseline py-3">
      <div class="w-1/2 h-auto">
        <p class="mt-5 text-lg text-gray-600">Customer Info</p>
        <div class="space-y-1">
          <div class="flex items-center gap-2">
            <label class="text-sm font-medium" for="name">Name : </label>
            <input v-model="data.customerInfo.name" type="text" id="name" class="border focus:outline-none px-2 inputBorder">
          </div>
          <div class="flex items-center gap-2">
            <label class="text-sm font-medium" for="email">Email : </label>
            <input v-model="data.customerInfo.email" type="email" id="email" class="border focus:outline-none px-2 inputBorder">
          </div>
          <div class="flex items-center gap-2">
            <label class="text-sm font-medium" for="phone">Phone : </label>
            <input v-model="data.customerInfo.phone" type="email" id="phone" class="border focus:outline-none px-2 inputBorder">
          </div>
        </div>
      </div>
      <div class="w-1/2 h-auto flex justify-between gap-10">
        <div class="flex flex-col w-1/2">
          <span class="text-lg text-gray-600">Billing Info</span>
          <textarea v-model="data.billingInfo" name="" id="" cols="30" rows="2" class="border focus:outline-none py-0.5 inputBorder"></textarea>
        </div>
        <div class="flex flex-col w-1/2">
          <span class="text-lg text-gray-600">Shipping Info</span>
          <textarea v-model="data.shippingInfo" name="" id="" cols="30" rows="2" class="border focus:outline-none py-0.5 inputBorder"></textarea>
        </div>
      </div>
    </div>
    <!--  Invoice Customer Info End  -->
    <!--  Invoice Product Info End  -->
    <div class="mt-10">
      <table class="table-auto w-full">
        <tr class="bg-gray-800 text-left text-white">
          <th class="p-2">S/L</th>
          <th class="p-2 pl-5 w-1/2">Item</th>
          <th class="p-2">Quantity</th>
          <th class="p-2">Rate</th>
          <th class="p-2 w-[200px] text-right pr-5">Amount</th>
          <th class="p-2 actionItem">Action</th>
        </tr>
        <tr v-for="(item, index) in data.items" :key="index">
          <td class="text-center">{{ item.sl = index + 1 }}</td>
          <td class="py-1">
            <input v-model="item.description" class="w-full pl-5 border focus:outline-none py-0.5 px-2 rounded inputBorder" type="text" placeholder="Description" />
          </td>
          <td class="">
            <input v-model="item.qty" class="w-full border focus:outline-none py-0.5 px-2 rounded inputBorder" type="number" placeholder="Quantity" />
          </td>
          <td class="">
            <!-- <input @input="updateAmount(item)" v-model="item.rate" class="w-full" type="number" placeholder="Rate"> -->
            <input v-model="item.rate" class="w-full border focus:outline-none py-0.5 px-2 rounded inputBorder" type="number" placeholder="Rate">
          </td>
          <td class="py-1 pr-5 text-right text-gray-800">
            $ {{ item.amount = item.qty * item.rate }}
          </td>
          <td class="text-center actionItem">
            <button @click="deleteItem(index)" class="btn bg-red-500 px-2 py-0.5 rounded text-white"><i class="fa-solid fa-xmark"></i></button>
          </td>
        </tr>
      </table>
      <button @click="addMoreItem()" class="btn mt-5 bg-green-600 hover:bg-green-700 text-white font-bold py-2 px-4 rounded">
        Add More
      </button>
      <button @click="getPrint()" class="btn ml-2 mt-5 bg-green-600 hover:bg-green-700 text-white font-bold py-2 px-4 rounded">
        Print
      </button>

    </div>
    <!--  Invoice Product Info End  -->
    <div class="mt-20">
      <div class="flex justify-between">
        <div class="flex flex-col space-y-5 w-1/2">
          <span>Notes</span>
          <textarea v-model="data.notes" name="" id="" cols="30" rows="2" class="border focus:outline-none py-0.5 px-2 rounded inputBorder"></textarea>
          <span>Terms</span>
          <textarea v-model="data.terms" name="" id="" cols="30" rows="2" class="border focus:outline-none py-0.5 px-2 rounded inputBorder"></textarea>
        </div>
        <div class="flex flex-col w-1/2 items-end">
          <div class="mt-10 flex-y-5 text-right space-y-1 w-full">
            <p>
              <span>Subtotal</span>
              <input :value="getSubTotal()" readonly class="focus:outline-none py-0.5 px-2 rounded text-right ml-2 pr-4 w-[200px]" placeholder="Subtotal">
            </p>
            <p>
              <span>Tax</span>
              <input v-model="data.tax" type="number" class="tax text-right w-[200px] ml-2 border focus:outline-none py-0.5 px-2 rounded inputBorder">
            </p>
            <p>
              <span>Total</span>
              <input :value="getTotal()" readonly class="focus:outline-none py-0.5 px-2 rounded text-right ml-2 pr-4 w-[200px]" placeholder="Total">
            </p>
            <p>
              <span>Payment</span>
              <input v-model="data.payment" class="tax text-right w-[200px] ml-2 border focus:outline-none py-0.5 px-2 rounded inputBorder" placeholder="Total Payment">
            </p>
            <p>
              <span>Balance Due</span>
              <input :value="getBalanceDue()" readonly class="focus:outline-none py-0.5 px-2 rounded text-right ml-2 pr-4 w-[200px]" :placeholder="data.balanceDue">
            </p>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
 @media print {
    .btn, .actionItem{
      display: none;
    }
   .inputBorder{
     border: none;
     outline: none;
   }
   textarea {
     resize: none;
   }

 }
</style>
