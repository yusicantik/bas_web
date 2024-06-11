<script setup>
import router from '@/router'
import { inject, reactive } from 'vue'
import { VNumberInput } from 'vuetify/labs/VNumberInput'

const dataTF = reactive({
  AccountID: '',
  BankID: '',
  Amount: 0,
  snackbar: false,
  pesanTF: ''
})

const myAxios = inject('myAxios')

const Tf = () => {
  console.log('Tf clicked', dataTF)

  myAxios
    .post('/transaction/transferbank', {
      AccountID: dataTF.AccountID,
      BankID: dataTF.BankID,
      Amount: dataTF.Amount,
      TransactionDate: dataTF.TransactionDate
    })
    .then(
      (res) => {
        if (res.status == 200) {
          dataTF.pesanTF = 'Transfer Anda Berhasil'
          router.push('transfer')
        }
        dataTF.snackbar = true
      },
      (err) => {
        dataTF.pesanTF = 'Gagal Transfer'
        dataTF.snackbar = true
      }
    )
}
</script>

<template>
  <v-card class="pa-2">
    <div class="container">
      <div>
        <label>Account ID</label>
        <v-text-field type="text" v-model="dataTF.AccountID" />
      </div>
      <div>
        <label>Bank ID</label>
        <v-text-field type="text" v-model="dataTF.BankID" />
      </div>
      <div>
        <label>Amount</label>
        <v-number-input type="text" v-model="dataTF.Amount" />
      </div>

      <div>
        <v-btn @click="Tf">Send Transfer</v-btn>
      </div>
      <v-snackbar v-model="dataTF.snackbar">
        {{ dataTF.pesanTF }}
        <template v-slot:actions>
          <v-btn color="pink" variant="text" @click="dataTF.snackbar = false"> Close </v-btn>
        </template>
      </v-snackbar>
    </div>
  </v-card>
</template>
