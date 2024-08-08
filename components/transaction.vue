<script setup lang="ts">

const props = defineProps({
  transaction: Object
})

const { currency } = useCurrency(3000)
const isLoading  = ref(false)
const toast = useToast()
const supabase = useSupabaseClient()



const emit = defineEmits(['deleted']) //silme işlemini yapmak için index.vue dosyasında kullanılmak üzere emit eventi yayınladık, index componentinde yakaladık.


const deleteTransaction = async () => {

 isLoading.value = true

 try{
  await supabase
  .from('transactions')
  .delete()
  .eq('id', props.transaction?.id)

  toast.add({
    title: 'Transactions deleted',
    icon: 'i-heroicons-check-circle',
    color: 'green'
  })
  emit('deleted',props.transaction?.id) //neyi sileceğimizi ve nerede sileceğimizi belirttik

 }catch(e){
  toast.add({
    title: 'Transactions not deleted',
    icon: 'i-heroicons-exclamation-circle',
    color: 'red'
  })
 }
 finally{
  isLoading.value = false
 }
  
}

const items = [
    [
      {
        label: 'Edit',
        icon: 'i-heroicons-pencil-square-20-solid',
        click: () => console.log('Edit')
      },
      {
        label: 'Delete',
        icon: 'i-heroicons-trash-square-20-solid',
        click: deleteTransaction
      }
    ]
]

const isIncome = computed(() => props.transaction?.type === 'Income')

const icon = computed(() => isIncome.value ? 'i-heroicons-arrow-up-right' : 'i-heroicons-arrow-down-left')

const iconColor = computed(()=> isIncome.value ? 'text-green-600': 'text-red-600')

</script>


<template>
  <div class="grid grid-cols-2 py-4 border-b border-gray-200 dark:border-gray-800 text-gray-100">
    <div class="flex items-center justify-between">
      <div class="flex items-center space-x-1">
        <UIcon :name="icon" :class="[iconColor]" />
        <div>{{ props.transaction?.amount }}</div>
      </div>

      <div>
        <UBadge color="white" v-if="transaction?.category">{{ transaction.category }}</UBadge>
      </div>
    </div>

    <div class="flex items-center justify-end space-x-2">
      <div>{{ currency }}</div>
      <div>
        <UDropdown :items="items" :popper="{placement: 'bottom-start'}">
          <UButton color="white" variant="ghost" trailing-icon="i-heroicons-ellipsis-horizontal" :loading = "isLoading"/>
        </UDropdown>
      </div>
    </div>
  </div>
</template>
