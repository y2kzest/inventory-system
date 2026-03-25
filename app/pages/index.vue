<template>
  <div class="min-h-screen bg-[#f8fafc] flex font-sans">
    <aside class="w-72 bg-slate-900 hidden lg:flex flex-col p-8 text-white border-r border-slate-800 shadow-2xl">
      <div class="flex items-center gap-3 mb-12">
        <div class="w-10 h-10 bg-emerald-500 rounded-xl flex items-center justify-center font-bold">A</div>
        <div class="flex flex-col">
          <span class="font-bold text-lg tracking-tight">A&A Cano</span>
          <span class="text-[10px] text-slate-400 uppercase tracking-widest">Construction Group</span>
        </div>
      </div>
      <nav class="space-y-2">
        <div class="flex items-center gap-3 text-emerald-400 bg-emerald-400/10 p-3 rounded-xl border border-emerald-400/20">
          <UIcon name="i-heroicons-squares-2x2" class="w-5 h-5" />
          <span class="font-medium">Inventory Tracker</span>
        </div>
      </nav>
    </aside>

    <main class="flex-1 flex flex-col min-w-0 overflow-hidden">
      <header class="h-20 bg-white border-b border-slate-200 flex items-center justify-between px-10">
        <h2 class="font-bold text-xl text-slate-800">Project Material Tracker</h2>
        <UAvatar src="https://ui-avatars.com/api/?name=Aaron&background=10b981&color=fff" size="sm" />
      </header>

      <div class="p-10 overflow-y-auto">
        <div class="grid grid-cols-1 md:grid-cols-2 gap-8 mb-10">
          <UCard class="ring-1 ring-slate-200 shadow-sm border-none">
            <p class="text-xs font-bold text-slate-500 uppercase">Total Items</p>
            <p class="text-3xl font-black text-slate-800">{{ inventory.length }}</p>
          </UCard>
          <UCard class="ring-1 ring-slate-200 shadow-sm border-none border-l-4 border-red-500">
            <p class="text-xs font-bold text-slate-500 uppercase">Over Budget</p>
            <p class="text-3xl font-black text-red-600">{{ overBudgetCount }}</p>
          </UCard>
        </div>

        <UCard class="shadow-sm border-none ring-1 ring-slate-200">
          <template #header>
            <div class="flex justify-between items-center py-2">
              <h3 class="font-bold text-slate-800">Active Materials List</h3>
              <div class="flex gap-3">
                <UButton label="Excel" icon="i-heroicons-document-arrow-down" color="emerald" variant="soft" @click="downloadExcel" />
                <UButton label="New Entry" icon="i-heroicons-plus" color="black" class="rounded-lg" @click="isModalOpen = true" />
              </div>
            </div>
          </template>

          <UTable :rows="inventory" :columns="columns">
            <template #difference-data="{ row }">
              <span :class="(row.budgeted - row.actual) < 0 ? 'text-red-600 font-black' : 'text-emerald-600 font-bold'">
                {{ row.budgeted - row.actual }}
              </span>
            </template>
            <template #actions-data="{ row }">
              <UButton variant="ghost" color="red" icon="i-heroicons-trash" @click="deleteItem(row)" />
            </template>
          </UTable>
        </UCard>
      </div>
    </main>

    <USlideover v-model="isModalOpen">
    <div class="p-8 flex flex-col h-full bg-white dark:bg-slate-900 min-w-[400px]">
      
      <div class="flex items-center justify-between mb-10">
        <h2 class="text-2xl font-black text-slate-800 dark:text-white">Add New Material</h2>
        <UButton color="gray" variant="ghost" icon="i-heroicons-x-mark" @click="isModalOpen = false" />
      </div>
      
      <div class="space-y-6 flex-1 overflow-y-auto pr-2">
        
        <UFormGroup label="Recording Date" class="text-slate-700 font-bold">
          <UInput v-model="form.date" type="date" size="lg" icon="i-heroicons-calendar" color="white" />
        </UFormGroup>

        <UFormGroup label="Pay Item / Ref #" class="text-slate-700 font-bold">
          <UInput v-model="form.item_number" placeholder="e.g. CANO-101" size="lg" icon="i-heroicons-hashtag" color="white" />
        </UFormGroup>

        <UFormGroup label="Material Description" class="text-slate-700 font-bold">
          <UInput v-model="form.description" placeholder="e.g. Portland Cement (40kg)" size="lg" icon="i-heroicons-cube" color="white" />
        </UFormGroup>

        <div class="grid grid-cols-2 gap-5">
          <UFormGroup label="Budgeted Qty" class="text-slate-700 font-bold">
            <UInput v-model.number="form.budgeted" type="number" size="lg" placeholder="0" color="white" />
          </UFormGroup>
          <UFormGroup label="Actual Qty" class="text-slate-700 font-bold">
            <UInput v-model.number="form.actual" type="number" size="lg" placeholder="0" color="white" />
          </UFormGroup>
        </div>

        <div class="mt-8 p-6 bg-slate-50 dark:bg-slate-800 rounded-2xl border border-slate-100 dark:border-slate-700">
          <p class="text-[10px] uppercase font-bold text-slate-400 tracking-widest mb-2">Auto-Calculation</p>
          <div class="flex justify-between items-center">
            <span class="text-sm font-semibold text-slate-600 dark:text-slate-300">Projected Variance:</span>
            <span :class="(form.budgeted - form.actual) < 0 ? 'text-red-600' : 'text-emerald-600'" class="font-black text-xl">
              {{ form.budgeted - form.actual }}
            </span>
          </div>
        </div>
      </div>

      <div class="pt-8 border-t border-slate-100 dark:border-slate-800 flex gap-4 mt-auto">
        <UButton label="Cancel" variant="ghost" color="gray" class="flex-1 hover:bg-slate-100" @click="isModalOpen = false" />
        <UButton label="Save Entry" color="black" class="flex-1 py-4 font-bold shadow-xl transition-transform active:scale-95" @click="saveItem" />
      </div>

    </div>
  </USlideover>
  </div>
</template>

<script setup>
import * as XLSX from 'xlsx'

const isModalOpen = ref(false)

const columns = [
  { id: 'date', key: 'date', label: 'Date' },
  { id: 'item_number', key: 'item_number', label: 'Ref #' },
  { id: 'description', key: 'description', label: 'Material' },
  { id: 'budgeted', key: 'budgeted', label: 'Budgeted' },
  { id: 'actual', key: 'actual', label: 'Actual' },
  { id: 'difference', key: 'difference', label: 'Variance' },
  { id: 'actions', key: 'actions', label: '' }
]

const inventory = ref([
  { id: 1, date: '2026-03-26', item_number: 'CANO-001', description: 'Portland Cement', budgeted: 500, actual: 480 }
])

const form = ref({
  date: new Date().toISOString().split('T')[0],
  item_number: '',
  description: '',
  budgeted: 0,
  actual: 0
})

const overBudgetCount = computed(() => {
  return inventory.value.filter(i => (i.budgeted - i.actual) < 0).length
})

function saveItem() {
  if (!form.value.description) return alert("Please enter a material description")
  inventory.value.push({ id: Date.now(), ...form.value })
  isModalOpen.value = false
  form.value = { date: new Date().toISOString().split('T')[0], item_number: '', description: '', budgeted: 0, actual: 0 }
}

function deleteItem(row) {
  inventory.value = inventory.value.filter(i => i.id !== row.id)
}

function downloadExcel() {
  const ws = XLSX.utils.json_to_sheet(inventory.value)
  const wb = XLSX.utils.book_new()
  XLSX.utils.book_append_sheet(wb, ws, "Inventory")
  XLSX.writeFile(wb, "Cano_Inventory.xlsx")
}
</script>