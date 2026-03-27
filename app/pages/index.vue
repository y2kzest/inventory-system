<template>
              <div class="grid grid-cols-1 gap-4 md:grid-cols-2">
                <label class="block">
                  <span class="mb-1 block text-sm font-medium text-slate-700">Supplier Email Address</span>
                  <input
                    v-model="form.supplier_email"
                    type="email"
                    placeholder="e.g. supplier@email.com"
                    class="w-full rounded-lg border border-slate-300 px-3 py-2 text-slate-900 outline-none transition focus:border-emerald-500 focus:ring-2 focus:ring-emerald-200"
                  >
                </label>
              </div>
  <div class="min-h-screen bg-slate-50">
    <div class="mx-auto max-w-7xl px-4 py-8 sm:px-6 lg:px-8">
      <header class="mb-8 flex items-center justify-between">
        <div>
          <h1 class="text-2xl font-bold text-slate-900">
            Material Inventory Tracker
          </h1>
          <p class="text-sm text-slate-600">
            Add a material below, then it will appear in the list instantly.
          </p>
        </div>
      </header>

      <section class="mb-8 rounded-2xl border border-slate-200 bg-white p-6 shadow-sm">
        <h2 class="mb-4 text-lg font-semibold text-slate-900">
          New Entry
        </h2>

        <form
          class="space-y-5"
          @submit.prevent="saveItem"
        >
          <div class="grid grid-cols-1 gap-4 md:grid-cols-2">
            <label class="block">
              <span class="mb-1 block text-sm font-medium text-slate-700">Date</span>
              <input
                v-model="form.date"
                type="date"
                class="w-full rounded-lg border border-slate-300 px-3 py-2 text-slate-900 outline-none transition focus:border-emerald-500 focus:ring-2 focus:ring-emerald-200"
              >
            </label>

            <label class="block">
              <span class="mb-1 block text-sm font-medium text-slate-700">Pay Item / Ref #</span>
              <input
                v-model="form.item_number"
                type="text"
                placeholder="e.g. CANO-101"
                class="w-full rounded-lg border border-slate-300 px-3 py-2 text-slate-900 outline-none transition focus:border-emerald-500 focus:ring-2 focus:ring-emerald-200"
              >
            </label>
          </div>

          <div class="grid grid-cols-1 gap-4 md:grid-cols-2">
            <label class="block">
              <span class="mb-1 block text-sm font-medium text-slate-700">Supplier Email Address</span>
              <input
                v-model="form.supplier_email"
                type="email"
                placeholder="e.g. supplier@email.com"
                class="w-full rounded-lg border border-slate-300 px-3 py-2 text-slate-900 outline-none transition focus:border-emerald-500 focus:ring-2 focus:ring-emerald-200"
              >
            </label>
          </div>

            <div class="grid grid-cols-1 gap-4 md:grid-cols-2">
              <label class="block">
                <span class="mb-1 block text-sm font-medium text-slate-700">Supplier Name</span>
                <input
                  v-model="form.supplier_name"
                  type="text"
                  placeholder="e.g. ABC Supply Co."
                  class="w-full rounded-lg border border-slate-300 px-3 py-2 text-slate-900 outline-none transition focus:border-emerald-500 focus:ring-2 focus:ring-emerald-200"
                >
              </label>
              <label class="block">
                <span class="mb-1 block text-sm font-medium text-slate-700">Supplier Address</span>
                <input
                  v-model="form.supplier_address"
                  type="text"
                  placeholder="e.g. 123 Main St, City"
                  class="w-full rounded-lg border border-slate-300 px-3 py-2 text-slate-900 outline-none transition focus:border-emerald-500 focus:ring-2 focus:ring-emerald-200"
                >
              </label>
            </div>

            <div class="grid grid-cols-1 gap-4 md:grid-cols-2">
              <label class="block">
                <span class="mb-1 block text-sm font-medium text-slate-700">Supplier Contact Number</span>
                <input
                  v-model="form.supplier_contact"
                  type="text"
                  placeholder="e.g. 0917-123-4567"
                  class="w-full rounded-lg border border-slate-300 px-3 py-2 text-slate-900 outline-none transition focus:border-emerald-500 focus:ring-2 focus:ring-emerald-200"
                >
              </label>
              <label class="block">
                <span class="mb-1 block text-sm font-medium text-slate-700">Project Name</span>
                <input
                  v-model="form.project_name"
                  type="text"
                  placeholder="e.g. Bridge Construction"
                  class="w-full rounded-lg border border-slate-300 px-3 py-2 text-slate-900 outline-none transition focus:border-emerald-500 focus:ring-2 focus:ring-emerald-200"
                >
              </label>
          </div>

          <label class="block">
            <span class="mb-1 block text-sm font-medium text-slate-700">Material Description</span>
            <input
              v-model="form.description"
              type="text"
              placeholder="e.g. Portland Cement (40kg)"
              class="w-full rounded-lg border border-slate-300 px-3 py-2 text-slate-900 outline-none transition focus:border-emerald-500 focus:ring-2 focus:ring-emerald-200"
            >
          </label>

          <div class="grid grid-cols-1 gap-4 md:grid-cols-2">
            <label class="block">
              <span class="mb-1 block text-sm font-medium text-slate-700">Budgeted Qty</span>
              <input
                v-model.number="form.budgeted"
                type="number"
                min="0"
                class="w-full rounded-lg border border-slate-300 px-3 py-2 text-slate-900 outline-none transition focus:border-emerald-500 focus:ring-2 focus:ring-emerald-200"
              >
            </label>

            <label class="block">
              <span class="mb-1 block text-sm font-medium text-slate-700">Actual Qty</span>
              <input
                v-model.number="form.actual"
                type="number"
                min="0"
                class="w-full rounded-lg border border-slate-300 px-3 py-2 text-slate-900 outline-none transition focus:border-emerald-500 focus:ring-2 focus:ring-emerald-200"
              >
            </label>
          </div>

          <div class="flex flex-wrap items-center justify-between gap-3 rounded-xl bg-slate-100 px-4 py-3">
            <p class="text-sm text-slate-700">
              Current variance:
              <strong :class="formVariance < 0 ? 'text-red-600' : 'text-emerald-700'">
                {{ formVariance }}
              </strong>
            </p>

            <div class="flex gap-2">
              <button
                type="button"
                class="rounded-lg border border-slate-300 bg-white px-4 py-2 text-sm font-semibold text-slate-700 hover:bg-slate-50"
                @click="resetForm"
              >
                Reset
              </button>
              <button
                type="submit"
                class="rounded-lg bg-slate-900 px-4 py-2 text-sm font-semibold text-white hover:bg-slate-800"
              >
                Save Entry
              </button>
            </div>
          </div>
        </form>
      </section>

      <section class="rounded-2xl border border-slate-200 bg-white p-6 shadow-sm">
        <div class="mb-4 flex flex-wrap items-center justify-between gap-3">
          <h2 class="text-lg font-semibold text-slate-900">
            Material List
          </h2>

          <div class="flex items-center gap-2">
            <span class="rounded-full bg-slate-100 px-3 py-1 text-xs font-semibold text-slate-700">
              Total: {{ inventory.length }}
            </span>
            <span class="rounded-full bg-red-100 px-3 py-1 text-xs font-semibold text-red-700">
              Over Budget: {{ overBudgetCount }}
            </span>
            <button
              type="button"
              class="rounded-lg bg-emerald-600 px-4 py-2 text-sm font-semibold text-white hover:bg-emerald-500"
              @click="downloadExcel"
            >
              Export Excel
            </button>
          </div>
        </div>

        <div class="overflow-x-auto">
          <table class="min-w-full border-collapse text-sm">
            <thead>
              <tr class="border-b border-slate-200 bg-slate-50 text-left">
                <th class="px-3 py-2 font-semibold text-slate-700">
                  Date
                </th>
                <th class="px-3 py-2 font-semibold text-slate-700">
                  Ref #
                </th>
                <th class="px-3 py-2 font-semibold text-slate-700">
                  Material
                </th>
                  <th class="px-3 py-2 font-semibold text-slate-700">
                    Supplier Name
                  </th>
                  <th class="px-3 py-2 font-semibold text-slate-700">
                    Supplier Address
                  </th>
                  <th class="px-3 py-2 font-semibold text-slate-700">
                    Contact #
                  </th>
                  <th class="px-3 py-2 font-semibold text-slate-700">
                    Email Address
                  </th>
                  <th class="px-3 py-2 font-semibold text-slate-700">
                    Project Name
                  </th>
                <th class="px-3 py-2 font-semibold text-slate-700">
                  Budgeted
                </th>
                <th class="px-3 py-2 font-semibold text-slate-700">
                  Actual
                </th>
                <th class="px-3 py-2 font-semibold text-slate-700">
                  Variance
                </th>
                <th class="px-3 py-2 font-semibold text-slate-700">
                  Actions
                </th>
              </tr>
            </thead>

            <tbody>
              <tr v-if="inventory.length === 0">
                <td
                  colspan="7"
                  class="px-3 py-8 text-center text-slate-500"
                >
                  No entries yet. Fill the form above and click Save Entry.
                </td>
              </tr>

              <tr
                v-for="item in inventory"
                :key="item.id"
                class="border-b border-slate-100"
              >
                <td class="px-3 py-2 text-slate-700">
                  {{ item.date }}
                </td>
                <td class="px-3 py-2 text-slate-700">
                  {{ item.item_number || '-' }}
                </td>
                <td class="px-3 py-2 text-slate-900">
                  {{ item.description }}
                </td>
                  <td class="px-3 py-2 text-slate-700">
                    {{ item.supplier_name || '-' }}
                  </td>
                  <td class="px-3 py-2 text-slate-700">
                    {{ item.supplier_address || '-' }}
                  </td>
                  <td class="px-3 py-2 text-slate-700">
                    {{ item.supplier_contact || '-' }}
                  </td>
                  <td class="px-3 py-2 text-slate-700">
                    {{ item.supplier_email || '-' }}
                  </td>
                  <td class="px-3 py-2 text-slate-700">
                    {{ item.project_name || '-' }}
                  </td>
                <td class="px-3 py-2 text-slate-700">
                  {{ item.budgeted }}
                </td>
                <td class="px-3 py-2 text-slate-700">
                  {{ item.actual }}
                </td>
                <td
                  class="px-3 py-2 font-semibold"
                  :class="item.difference < 0 ? 'text-red-600' : 'text-emerald-700'"
                >
                  {{ item.difference }}
                </td>
                <td class="px-3 py-2">
                  <button
                    type="button"
                    class="rounded-md border border-red-300 px-3 py-1 text-xs font-semibold text-red-700 hover:bg-red-50"
                    @click="deleteItem(item.id)"
                  >
                    Delete
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </section>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, onMounted, reactive, ref } from 'vue'



type InventoryItem = {
  id: number
  date: string
  item_number: string
  description: string
  supplier_name: string
  supplier_address: string
  supplier_contact: string
  supplier_email: string
  project_name: string
  budgeted: number
  actual: number
  difference: number
}


type MaterialForm = {
  date: string
  item_number: string
  description: string
  supplier_name: string
  supplier_address: string
  supplier_contact: string
  supplier_email: string
  project_name: string
  budgeted: number
  actual: number
}

const inventory = ref<InventoryItem[]>([])

const form = reactive<MaterialForm>({
  date: '',
  item_number: '',
  description: '',
  supplier_name: '',
  supplier_address: '',
  supplier_contact: '',
  supplier_email: '',
  project_name: '',
  budgeted: 0,
  actual: 0
})

const overBudgetCount = computed(() => {
  return inventory.value.filter(item => item.difference < 0).length
})

const formVariance = computed(() => {
  return (Number(form.budgeted) || 0) - (Number(form.actual) || 0)
})

function getTodayDate() {
  return new Date().toISOString().split('T')[0] || ''
}

function resetForm() {
  form.date = getTodayDate()
  form.item_number = ''
  form.description = ''
  form.supplier_name = ''
  form.supplier_address = ''
  form.supplier_contact = ''
  form.supplier_email = ''
  form.project_name = ''
  form.budgeted = 0
  form.actual = 0
}

function saveItem() {
  const description = form.description.trim()
  if (!description) {
    alert('Please enter a material description')
    return
  }

  const budgeted = Number(form.budgeted) || 0
  const actual = Number(form.actual) || 0

  inventory.value.push({
    id: Date.now(),
    date: form.date || getTodayDate(),
    item_number: form.item_number.trim(),
    description,
    supplier_name: form.supplier_name.trim(),
    supplier_address: form.supplier_address.trim(),
    supplier_contact: form.supplier_contact.trim(),
    supplier_email: form.supplier_email.trim(),
    project_name: form.project_name.trim(),
    budgeted,
    actual,
    difference: budgeted - actual
  })

  resetForm()
}

function deleteItem(id: number) {
  inventory.value = inventory.value.filter(item => item.id !== id)
}

async function downloadExcel() {
  if (typeof window === 'undefined') {
    return
  }

  const XLSX = await import('xlsx')
  const ws = XLSX.utils.json_to_sheet(inventory.value)
  const wb = XLSX.utils.book_new()

  XLSX.utils.book_append_sheet(wb, ws, 'Inventory')
  XLSX.writeFile(wb, 'Cano_Inventory.xlsx')
}

onMounted(() => {
  resetForm()
})
</script>
